# while loop
In for loop, we used arithmetic expression to evaluate the condition. Generally, in while loop we'll use test command to evaluate the condition.
```cmd
# while((i--)); do echo $i;done; echo we are done
we are done
```
Initially value of i when not mentioned is 0, thus `i--=0`.
But if we repeat the loop again, we'll see infinite looping.
# Random Value
There is a variable called RANDOM.

```cmd
# echo $RANDOM
23940
# echo $RANDOM
10705
# echo $RANDOM
6004
```
## Seed
We can seed value for RNG as well.
```cmd
# RANDOM=217
# echo $RANDOM
9871
```
## Value between 0-9
Use modulus operator to get remainder.
```cmd
# echo $(($RANDOM%10))
9
```
`%` is arithmetic operator so we need `(( ))` this to put.

# while loop and random events
```cmd
#!/bin/bash
i=20
while ((i > 0)); do
    ((rand = RANDOM % 10))
    ((rand > 6)) && ((i++)) || ((i--))
    echo rand:$rand i:$i
    sleep .7s
done
echo we are done
```
Output
```cmd
rand:7 i:21
rand:1 i:20
rand:6 i:19
rand:5 i:18
rand:7 i:19
rand:8 i:20
rand:8 i:21
rand:2 i:20
rand:9 i:21
rand:6 i:20
rand:2 i:19
rand:7 i:20
rand:6 i:19
rand:5 i:18
rand:1 i:17
rand:3 i:16
rand:9 i:17
rand:1 i:16
rand:7 i:17
rand:7 i:18
rand:6 i:17
rand:2 i:16
rand:0 i:15
rand:2 i:14
rand:8 i:15
rand:3 i:14
rand:5 i:13
rand:8 i:14
rand:8 i:15
rand:8 i:16
rand:4 i:15
rand:8 i:16
rand:7 i:17
rand:9 i:18
```
# lockfile
```cmd
#!/bin/bash
i=0
while
    echo +++++++++
    ((i++))
    echo -n "checking for file: "
    ls lockfile
do
    echo --------
    echo $i - file still exists
    echo going to sleep
    sleep .1
    echo back from sleep
    echo --------
done
echo "%%%%%%%%%%%%%%%%%%%%--$i" >>datafile
```
lockfiles are used to co-ordinate access to shared data between coordinating processes. So, one process is going ahead to access some data, but it wants to tell other processes it''s busy within a certain resource. So, it creates a lock file.

Name can be any file name. And all the other processes they wait before they access the same data until the file disappears.

This code is written in very unintuitive way. 

# implementation of lockfile
```cmd
#!/bin/bash
i=300
while ((i > 0)); do
    touch lockfile
    sleep .2
    echo -n $i >>datafile
    sleep .3
    echo -n " xx "$i >>datafile
    sleep .3
    echo " xxxx " $i >>datafile
    rm lockfile
    cat datafile
    echo --
    ((i--))
    sleep .2
done
```
Problem with this:
Linux actually buffers data writes to disk. So, when I say echo this piece of data to data file, I can't be sure that exactly in that moment that piece of data is written to disk.
# until loop
## exit status of looping
Looping construct is a compound command. Every command in Linux has an exit status. So, also the while loop, for loop and until loop have an exit status.
The return status is the exit status of the last command executed in the consequent commands.
So, the exit status of the last command in the loop body is the exit status of the entire looping construct. Or, if we never enter the loop, we get zero as an exit status.

until-loop functions exactly like the while-loop, just the other way around.

The only difference here is, we enter the until-loop if the initial test is false.

```cmd
until test-commands; do
    consequent-commands
done
```


So, until test command becomes true, we loop. As soon as tis becomes true, the looping ends. Until this becomes true, means as long as test-command is false.

So, if you've 1 command, the exit status of that command should be false, and if you have a list of commands the exit status of the last command in the list of commands should be false; And if that's the case, we loop, otherwise we don't.

So, that's exactly the opposite from while.

In while loop, we loop if exit status is true.
In until, we loop if exit status is false.

Everything else is completely identical.

We never really use a while loop.

## equivalence of while loop and until loop
```cmd
while loop
#!/bin/bash
i=10
while ((i--)); do
    echo $i
done
```

```cmd
until loop
#!/bin/bash
i=10
until !((i--)); do
    echo $i
done
```

So, to use until loop just inverse the condition of while loop.
