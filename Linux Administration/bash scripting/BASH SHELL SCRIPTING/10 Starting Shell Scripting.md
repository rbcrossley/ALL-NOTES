# 1 
```cmd
#!/bin/bash
```
is called shebang.
Why?
`#` is called sharp in music.
`!` is called bang in linux.

```
for i in one two three; do
	echo $i
done
```

## Execute Script
There are multiple ways to execute this script.
a) Using `bash scriptname`
b) source ./scriptname
c) .`./scriptname`

a) Using `bash scriptname`
No need to make the script executable.
We start a new bash shell in this case.
Bash looks for file in current directory so no need to specify the current directory.
A non interactive shell is opened. A shell is interactive when you can type commands and get feedback. In non interactive shell, as soon as the file terminates, bash exits and we are back in this shell where we started.

b) `source ./scriptname`
No need to make the script executable.
We don't start a new shell in this case.
We need to specify `./scriptname`, bash looks in the system path.

c) .`/scriptname`
`chmod u+x scriptname`
This is totally identical to the first one. What happens here is exactly the same. So here also a new shell is started and the script is executed in the new shell. And then after it terminates the shell terminates and we're back to our original shell.

## Exit Status
If I do nothing extra, the exit status of that script will be the exit status of the last command executed within this script. If the last command succeeds, the whole command is said to be successful and thus its exit status is 0.

```cmd
[root@localhost ~]# vi print_i.sh
#!/bin/bash
for i in one two three; do
        echo $i
done
exit 19
```

Script will succeed but the exit status is non-zero. Shell could be confused if this is error because of non-zero exit status.

# 2 Script with command line arguments
```cmd
#!/bin/bash
echo $1
echo $2
echo $3
echo $4
echo $5
echo $6
echo $7
echo $8
echo $9
echo $10
echo $11
echo $12
echo $13
echo $14
```

```cmd
[root@localhost ~]# bash print_one_to_ten.sh one two three four five six seven eight nine ten eleven
one
two
three
four
five
six
seven
eight
nine
one0
one1
one2
one3
one4
```
The gist is that `$10`, shell will think it's `$1` then `0`.
Which would be equal to "one1".

So, always use curly braces around two digit numbers.
Like this.
`echo ${10}`

## Three Useful Variables
```cmd
1)#
2)@
3)*
```
For now, think  `@` and `*` are the same. When for loops chapter comes, it'll be more clear on what's the difference between these two.
`#` is a variable which gives number of command line arguments.

`@` is a variable that gives all command line arguments at once.
Examples:


```cmd
echo "The number of command line arguments are: $#"

[root@localhost ~]# bash hash_meaning.sh
The number of command line arguments are: 0
```

```cmd
echo "The all command line arguments are: $@"

[root@localhost ~]# bash all_command_line_args.sh 1 2 3 4 5 6 7 8 9 10
The all command line arguments are: 1 2 3 4 5 6 7 8 9 10
```

```cmd
[root@localhost ~]# bash at_and_hash.sh  a b c d e f g h
$#: 8
$@: a b c d e f g h
[root@localhost ~]# cat at_and_hash.sh
#!/bin/bash
echo \$#: $# #the count of command line arguments
echo \$@: $@ #all the command line argument

```

## Questions To Ponder
What does shift command does in bash?
How is it useful in shell scripting?