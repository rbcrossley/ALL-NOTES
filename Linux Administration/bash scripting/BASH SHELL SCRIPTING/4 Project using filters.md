 # tr
 Translates Strings
.

`tr aeiou 12345`

Will translate

```
a->1
e->2
i->3
o->4
u->5
```

Always input fed through stdin, not through file. (Not too sure about this.)

`tr [OPTION]... SET1 [SET2]`

If SET2 is shorter than SET1, SET2 is extended until it reaches the size of SET1 by repeating the last character.

If SET1 is shorter than SET2, excessive numbers in SET2 are useless for `tr`, nothing required.

## Delete
`tr -d 12345`

Deletes all characters that are  in SET1 no specifications of SET2 required.

```
Alphabet Survive,
Numbers Killed.
```

### Removes spaces from a file

`cat spaces_and_newlines | tr -d " "`

The output isn't very readable as all the spaces are removed.
### Squeeze a file
`cat spaces_and_newlines | tr -s " "`

The output is very readable, because 1 space is kept and all the redundant spaces are removed.

`cat spaces_and_newlines | tr -s "\n"`

All extra newlines are removed.

## Character Classes

`tr -d [:alpha:]`
will remove all alphabetical characters from the input

`tr -d [:digit:]`
will remove all digits.

`tr[:lower:][:upper:]`

will convert lowercase to uppercase. If already in uppercase, doesn't do anything.

# tr command elaborated 

## How to get the original value before translated using tr command?
```cmd
[root@load-balancer ~]# echo "hello friend" | tr hellofriend 12345678901
19445 678901
[root@load-balancer ~]# echo "19445 678901" | tr  12345678901 hellofriend
dello friend
```
Here I got dello friend because d and h have same value.
It won't happen always

## fold command
folds lines.
`-w` use width columns instead of 80.

`man fold | fold -w40`
40 characters per line will be generated.

`man fold | fold -w1`
1 character per line will be generated.

## shuf command

This shuffles. It works on new line only.

Create a file with numbers from 2 to 12, one number on each line (dice roll possibilities), and save it as dice.txt.

```cmd
2
3
4
5
6
7
8
9
10
11
12
```
Then run the following `shuf` command to extract one random number from the list, as if you are rolling dice:
```
$ shuf -n 1 dice.txt 
3
$ shuf -n 1 dice.txt 
7
$ shuf -n 1 dice.txt 
5
$ shuf -n 1 dice.txt 
12
$ shuf -n 1 dice.txt 
8
$ shuf -n 1 dice.txt 
7
$ shuf -n 1 dice.txt 
3
$ shuf -n 1 dice.txt 
11
```

Generate a random permutation of numbers from 1 to 10:
`shuf -i 1-10`
Output
```cmd
7
1
2
9
10
3
4
5
6
8
```
I've used this in this context.

`cat SET1 | fold -w1 | shuf | tr -d "\n" | wc -c`

If SET1 and SET2 contains spaces.

`cat tr_manual.txt | tr "$(cat SET1)" "$(cat SET2)"`

`tr` translates or delete characters from input

## fold
`fold -w1` means 1 character in 1 line. width of column=1 character.
