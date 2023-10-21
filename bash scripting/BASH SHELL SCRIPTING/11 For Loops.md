# Simple For Loop
```cmd
for each in 1 2 3 4 5; do
    echo "ABCD"
done

# bash ABCD.sh
ABCD
ABCD
ABCD
ABCD
ABCD
```
# Check Execution Permission
## conditional operator
```cmd
[[ $(ls) ]] &&echo "file is having execution permission"||echo "file is not having execution permission"
```
## if else statements
```cmd
for each in $(ls); do
    if [[ -x $each ]]; then
        echo "$each is having execution permission"
    else
        echo "$each is not having execution permission"
    fi
done

# bash ifelse.sh
ABCD.sh is not having execution permission
anaconda-ks.cfg is not having execution permission
execution1.sh is not having execution permission
ifelse.sh is not having execution permission
results is not having execution permission
```
# Use given path
```cmd
#!/bin/bash
given_path=$1
for each in $(ls $given_path); do
    if [[ -x $each ]]; then
        echo "$each is having execution permission"
    else
        echo "$each is not having execution permission"
    fi
done
```
`bash iffi.sh /path/to/directory`

# Check if you're passing the parameters or not
```cmd
#!/bin/bash
if [[ $# -ne 1 ]]; then
    echo "Use path"
    exit
fi
given_path=$1
for each in $(ls $given_path); do
    if [[ -x $each ]]; then
        echo "$each is having execution permission"
    else
        echo "$each is not having execution permission"
    fi
done

If I run without any path in script, I'll get
"Use path" as output.
```

# Different ways to use for loop
## Way-1
### Syntax
```cmd
for variable in list_of_values;
do
	command1
	command2
done
```

```cmd
#!/bin/bash
for each_value in 1 2 3; do
    echo"this is a loop"
done

# bash basicfor.sh
this is a loop
this is a loop
this is a loop
```
To print the value of the loop control variable.
```cmd
#!/bin/bash
for each_value in 1 2 3; do
    echo "this is a loop"
    echo "for this iteration each_value is: $each_value"
done
```
To use something else instead of 1,2,3
```cmd
for each_file in $(ls); do
    echo "This is a loop"
    echo "For this iteration each_value is: $each_file"
done

# bash repeatdir.sh
This is a loop
For this iteration each_value is: ABCD.sh
This is a loop
For this iteration each_value is: anaconda-ks.cfg
This is a loop
For this iteration each_value is: basicfor.sh
This is a loop
For this iteration each_value is: execution1.sh
This is a loop
For this iteration each_value is: givenpath.sh
This is a loop
For this iteration each_value is: ifelse.sh
This is a loop
For this iteration each_value is: repeatdir.sh
This is a loop
For this iteration each_value is: results
```
**What does `*` mean in for loop syntax?**
It means all the files in current directory.
```cmd
#!/bin/bash
for name in *; do
    echo "$name"
done

# bash asterik.sh
ABCD.sh
anaconda-ks.cfg
asterik.sh
basicfor.sh
cnt.sh
execution1.sh
givenpath.sh
ifelse.sh
repeatdir.sh
results
```
**To create a file with a space, just escape the space**
`echo hello world > hello\ world`
**Better to use double quotes always in variable name**
```cmd
#!/bin/bash
for name in *; do
    echo listing for: "$name"
    ls -ld "$name"
    echo -n "word count:" #-n means don't start another command output in new line
    wc -w <"$name"
    echo --------------------
done
```

**Empty Lists can occur when the list is created dynamically**
```cmd
#!/bin/bash
for name in $(ls new); do
    echo "$name"
done
```

```cmd
#!/bin/bash
for name; do
    echo "$name"
done
```

```cmd
#!/bin/bash
for name; do #semi-colon is optional
    echo "$name"
done

bash pass-param-loop.sh 1 2 3 4 5 6 7 8 9
1
2
3
4
5
6
7
8
9
```


## Way-2 C-Language type for loop
No need to declare a variable.
```cmd
echo $i
(( i++ ))
echo $1
1
(( i++ ))
echo $1
2
```
```cmd
for (( initialization;condition ; increment/decrement ))
do
	command1
	command2
done
```
OR
```cmd
#!/bin/bash
for ((expr1; expr2; expr3)); do
    commands
done
```

Example:
```cmd
#!/bin/bash
for ((cnt = 1; cnt <= 10; cnt++)); do
    echo "this is c type for loop"
done

# bash cnt.sh
this is c type for loop
this is c type for loop
this is c type for loop
this is c type for loop
this is c type for loop
this is c type for loop
this is c type for loop
this is c type for loop
this is c type for loop
this is c type for loop

```
**Simple for loop example**
```cmd
#!/bin/bash
for ((i = 0; i < 10; i++)); do
    echo $i
done
```
## Infinity for loop
```cmd
#!/bin/bash
echo "This is a for loop"
for (( ; ; )); do
    echo "This is infinity loop"
    sleep 1
done
```
Stopping this  loop logically is possible.
```cmd
#!/bin/bash
echo "This is a for loop"
cnt=1
for (( ; ; )); do
    echo "This is infinity loop"
    ((cnt++))
    sleep 1
    if [[ $cnt -eq 10 ]]; then
        break
    fi
done
```
### If we don't use exp1, it'll be assigned to 0 later.
```cmd
#!/bin/bash
for (( ; i<10;i++ ));do
	echo $i
done

```
### Interesting thing to notice is that first output will be nothing instead of 0. 
Because i didn't exit at that moment. It was created after that line was executed.
```cmd
# bash echoi.sh

1
2
3
4
5
6
7
8
9

```
Here i will be initialized to 0.

### expr3 is optional
```cmd
#!/bin/bash
i=3
for (( ; i < 10; )); do
    echo $i
    ((i++))
done

# bash noexpr
3
4
5
6
7
8
9
```

### expr2 is optional
That will be evaluated to true It's a way to create an infinite loop
```cmd
#!/bin/bash
i=3
for (( ; ; )); do
    echo $i
	sleep .3 #every 0.3 seconds, i is incremented
	(( i>5 ))&& break 
    ((i++))
done

# bash for123
3
4
5
6
```

# For loops in practice: Wrapper Script
The script will be written in phases
## Phase-1
```cmd
#!/bin/bash
# mangle set1 set2 file
SET1=$(cat $1)
SET2=$(cat $2)

echo $SET1
echo $SET2

output
whatever is in SET1 and SET2. They contain a long text of one line.
```
## Phase-2
```cmd
#!/bin/bash
# mangle set1 set2 file
SET1=$(cat $1)
SET2=$(cat $2)

echo $SET1
echo $SET2
```

## Phase-3
```cmd
#!/bin/bash
# mangle set1 set2 file
SET1=$(cat $1)
SET2=$(cat $2)

echo $SET1
echo $SET2

shift 2
i=0
for file; do
    echo "reading from file: " "$file"
    echo "Writing to file: " "$file.MANGLED"
    cat $file | tr "$SET1" "$SET2" >"$file.MANGLED"
    ((i++))
done

echo
echo $i files processed
```
**Another way of writing the same script**
```cmd
max=$#
for ((i = 0; i < $max; i++)); do
    echo "reading from file: " "$1"
    echo " writing to file: " "$1.MANGLED"
    cat "$1" | tr "$SET1" "$SET2" >"$1.MANGLED"
    shift
done
echo $1 files processed
```
I haven't understood this yet.
# Questions to Ponder
- What's the difference between writing `$(ls)` vs `${ls}` vs `$[ls]`