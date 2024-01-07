# if-statement part 1
```cmd
ls filename 2>&- >/dev/null && echo The file exists. || echo The file does not exist.
```

## Example of if statement
```cmd
#!/bin/bash
read num
if ((num < 10)); then
    echo $num is less than 10
elif ((num > 10)); then
    echo $num is greater than 10
else
    echo $num is exactly 10
fi
```

```cmd
23
23 is greater than 10
```

## simple if statement
```cmd
#!/bin/bash
read max
while read -p "$max, next number:" number; do
    if ((number > max)); then
        ((max = number))
    fi
done
echo $max
```
The script computes the maximum of numbers entered on the keyboard. Read first number. First number is maximum number because there's only one.
We read the next number.
Tells current maximum number and asks for the next number. So, we read next number into the variable called number and if number is greater than maximum then we replace maximum with that new number. Otherwise, we do nothing.
`Ctrl+D` loop ends and maximum of all numbers entered appears.
```cmd
#!/bin/bash
read max
while read -p "$max, next number:" number; do
    if ((number > max)); then
        ((max = number))
    fi
done
echo $max
```

## elif
```cmd
#!/bin/bash
read min
max=$min
while read -p "$min $max, next number: " number; do
    if ((number > max)); then
        max=$number
    elif ((number < min)); then
        min=$number
    fi
done
printf "%d %d\n" $min $max
```

```cmd
# bash minmax
1
3
0
0 3
```

**Tip: If the number starts with 0, bash consists it as octal number**
```cmd
printf "%o\n" 2046307
```
This will print the octal of this decimal.
## `2>&1`, `2>&-` and `2>&1-`
A1)
```cmd
command 2>&1 >somefile
```
Here stderr goes to stdout(terminal), later stdout goes to somefile.

A2)
```cmd
command > somefile 2>&1
```
stderr and stdout both go to same file.

A3) 
```cmd
ls 2>&-
```
stderr closed, stdout on terminal

A4) 
```cmd
2>&1-
```
stderr goes to stdout and later stdout closes. So both closed.
# if statement part 2
```cmd
#!/bin/bash

# Wrapper script to check whether a password complies to a set of rules.
# (delegates the check to validpwd)
#
# A password is valid if it conforms to the following rules:
#
#   - It has at least one lowercase character
#   - It has at least one uppercase character
#   - It has at least one digit
#   - It has at least one non-alphanumeric character

while read -rp "enter password candidate: "  password ; do
	./validpwd "$password"
	echo ----------
done
```

```cmd
#!/bin/bash

print=1		# print status messages
 
if (( $# == 0 )) || test "$1" == "?" || test "$1" == "-?" || test "$1" == "--help" ; then
	printf "%s\n" \
		"" \
		"NAME" \
		"	validpwd - validate password candidate" \
		"" \
		"SYNOPSIS" \
		"	validpwd [-s] PASSWORD" \
		"" \
		"DESCRIPTION" \
		"	A password qualifies if it has:" \
		"	   - at least one lowercase character AND" \
		"	   - at least one uppercase character AND" \
		"	   - at least one digit AND" \
		"	   - at least one non-alphanumeric character" \
		"" \
		"	-s	silent mode" \
		"" \
		"	-?, ?, --help" \
		"		display this help and exit" \
		"" \
		"EXIT STATUS" \
		"	0	if the candidate qualifies " \
		"	1	otherwise" \
		""
	exit 0
elif test "$1" == "-s" ; then
		print=0
		shift
		if (( $# == 0 )) ; then
			exit 1
		fi
fi

passwd="$1"

nonalphanum="\\\=\!\"§$%&/()?\`*+'#\-_:.,;{[]}^°~|<>@"
#echo $nonalphanum
problems=0

nodigits=0
#echo ----nodigits: "$passwd" "$(echo "$passwd" | tr -d [:digit:])"
if test "$(echo "$passwd" | tr -d [:digit:])" == "$passwd" ; then
	nodigits=1 ; (( problems++ ))
fi

nolowers=0
#echo ----nolowers: "$passwd" "$(echo "$passwd" | tr -d [:lower:])" 
if test "$(echo "$passwd" | tr -d [:lower:])" == "$passwd" ; then
	nolowers=1 ; (( problems++ ))
fi

nouppers=0
#echo ----nouppers: "$passwd" "$(echo "$passwd" | tr -d [:upper:])"
if test "$(echo "$passwd" | tr -d [:upper:])" == "$passwd" ; then
	nouppers=1 ; (( problems++ ))
fi

noothers=0
#echo ----noothers: "$passwd" "$(echo "$passwd" | tr -d "$nonalphanum")"
if test "$(echo "$passwd" | tr -d "$nonalphanum")" == "$passwd" ; then
	noothers=1 ; (( problems++ ))
fi

if (( problems > 0 && print == 1)) ; then
	if (( problems == 1 )) ; then
		str="is 1 problem"
	else 
		str="are $problems problems"
#	elif (( problems == 2 )) ; then
#		str="are two problems"
#	else
#		str="are three problems"
	fi

	printf "There %s with the password '%s':\n" "$str" "$passwd"

	if (( nodigits )) ; then echo $passwd contains no digits ; fi
	if (( nolowers )) ; then echo $passwd contains no lowercase characters ; fi
	if (( nouppers )) ; then echo $passwd contains no uppercase characters ; fi
	if (( noothers )) ; then echo $passwd contains no non-alphanumeric characters ; fi
elif (( print == 1 )) ; then
	printf "'%s' qualifies\n" $passwd
fi

if (( problems )) ; then 
	exit 1
fi
exit 0
```

I want to test if a password candidate conforms to a set of 4 rules as mentioned above.
Initially check if there've been any command line arguments, if not print man pages type.
I can provide `./validpwd --help` or `./validpwd -?`.
`test` just compares 2 strings.
Backslashes escape the new line.
They're all immediately followed by a newline.
Otherwise, I'd have to put the whole thing into 1 line. If `$1==-s` we go to elif clause and print signals that user wants to have screen output. Up here we `print=1`. If `-s` not specified, we'll see messages on the screen but `-s` turns it off and validpwd will only set an exit code.
The exit status is 0 if a candidate qualifies or 1 if it doesn't qualify. So, with `-s` option, I get no output at all and only a exit status and I use the value of print 1 or 0 to determine what the user wanted, what the user specified.
shift and check if any command line parameters left.
If number of parameters=0, it means nothing is given as parameters. 
Although this doesn't make any sense(but we should know that user is dumb). Because of nothing to do, the script exits with a 1 because there was no password that qualified. At last we're sure passwd=$1 because otherwise, it'd have exited.
problem counts problems with password.
We test for digits. No problems with digits means nodigits=0. It means that there were digits in password candidate.
Again, I use test command.
test returns exit code.
0->if 2 strings match.
greater than 0-> otherwise
`tr -d[:digit:]` tr to remove all digit from the password candidate. If the result of removing digits=input, it means there were no digits in the password candidate.
Thus, we make nodigits=1. And count up the problems variable. Do the same for lowercase characters.
Same goes for uppercase characters. Same goes for non-alphanumeric characters. If there were problems, we've to say so but only if the user wants screen output. So, if there were problems and user wants screen output, then print will be 1. This is arithmetic statement.
We just say "is 1 problem" here. Now print using that str variable. Depending on what's the problem, print out one message for each. if there were no problems, we don't go if clause.
If there were no problems but the user wants screen output, then I print to the screen that the password qualifies and that is that.
If not, we don't print anything to the screen. If that was silent moe, then print would be at zero so we would not get these messages and we would not get these messages. But we'll have an exit status. So, if there were problems, the exit status has to be 1 and we exit from here.
If there were no problems, then we reach this point in the script and we exit with zero. Looks daunting at first but very straightforward you can add check for length. 
Silent mode, `./validpwd -s password` . No screen output for password.
Not silent mode will gets output.
# case statement
```cmd
#!/bin/bash
while read WORD; do
    case $WORD in
    1*) echo -1 ;;
    2*) echo - 2 ;;
    3*) echo - 3 ;;
    4) echo - 4 ;;
    5) echo - 5 ;;
    6) echo - 6 ;;
    7) echo - 7 ;;
    8) echo - 8 ;;
    9) echo - 9 ;;
    0) echo - 0 ;;
    *) echo - something else ;;
    esac 
done
```

```cmd
#!/bin/bash
while read WORD; do
    case $WORD in
    1)
        ((one++))
        echo -1
        ;;
    2)
        ((two++))
        echo - 2
        ;;
    3)
        ((three++))
        echo - 3
        ;;
    4) echo - 4 ;;
    5) echo - 5 ;;
    6) echo - 6 ;;
    7) echo - 6 ;;
    8) echo - 6 ;;
    9) echo - 6 ;;
    0) echo - 6 ;;
    *) echo - something else ;;

    esac
done

echo one:$one two:$two three:$three
```
I put this just for knowing the documentation of these commands whenever I want to learn these.

# case example
```cmd
#!/bin/bash
while read -r LINE; do
    for word in $LINE; do
        echo $word
    done
done
```

Output
```cmd
[root@localhost case_statement]# bash 1.sh
my name is nepal
my
name
is
nepal
<enter more lines>
```


```cmd
[root@localhost case_statement]# echo my name is nepal | bash 1.sh
my
name
is
nepal
```
Below `@` is just the name of the file.
```cmd
#sort1
#!/bin/bash
while read -r LINE; do
    for word in $LINE; do
        case $word in
        A*) echo $word >>a ;;
        a*) echo $word >>a ;;
        *) echo $word >>@ ;;
        esac
    done
done
```
This is the template of the final script.
```cmd
#final_sort.sh
#!/bin/bash
while read -r LINE; do
    for word in $LINE; do
        case $word in
        A* | a*) echo $word >>a ;;
        [Ee]*) echo $word >>e ;;
        T*) echo $word >>t ;;
        t*) echo $word >>t ;;
        *) echo $word >>@ ;;
        esac
    done
done
```
To remove all single named filenames
`rm ?`
`mv ? bkp` to send all one words file to bkp directory.
`uniq` to remove duplicate words.
# script to generate code
Will do later