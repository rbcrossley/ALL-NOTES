# Rule of thumb
When in doubt, use single quotes.
# Single quotes
Shell explicitly told to not interfere with the string at all.
# Double quotes
Some shell substitution will take place
- variable
- tilde expansion
- command substitution
Before executing, shell rewrites and parses the command.
`ls *` is same as doing `ls first_file second_file third_file first_folder second_folder third_folder`
![](_resources/Pasted%20image%2020240201222657.png)
![](_resources/Pasted%20image%2020240201223037.png)
`echo *` prints all the items in the current directory.
![](_resources/Pasted%20image%2020240201223156.png)
Remember, unless specified otherwise:
```
~="${HOME}"
```
Why I say unless specified otherwise is because, I can set the `HOME=/` and `~` and the variable `HOME` would produce the different output than shown above.
![](_resources/Pasted%20image%2020240201223459.png)
It is like I am doing `ls /`. To further verify it, we can echo it.
![](_resources/Pasted%20image%2020240201223559.png)
```
~+ = $PWD
```
![](_resources/Pasted%20image%2020240201224104.png)
Remember these two are not the same thing. The first one is enclosed within braces whereas second one is not and that alone is creating a huge difference.
# Shell parameter expansion
![](_resources/Pasted%20image%2020240201224343.png)
Here the first command gives the length of the variable PATH.
# Summary so far
![](_resources/Pasted%20image%2020240316174745.png)
# Variable and Parameter Expansion
Variable expansion starts with dollar sign.
```
echo $HOME
echo ${HOME}
echo "$HOME"
echo "${HOME}"
```
Prefer double quotes and curly braces.
# Word splitting
- happens on the basis of IFS variable.
- By default, IFS is space, tab and newline.
```
[root@localhost ~]# echo "${IFS}"


[root@localhost ~]# echo "${IFS}"|hexdump
0000000 0920 0a0a
0000004
[root@localhost ~]#
```
- Sequences of IFS character are treated as a single delimiter.
```
touch a.txt b.txt 
touch a.txt                b.txt
```
Both commands are equivalent.
## Disable word splitting
- Put the parameters in a quote.
```
[root@localhost ~]# touch 'a file.txt'
[root@localhost ~]# ls
'a file.txt'   tori.txt
[root@localhost ~]# touch "b file.txt"
[root@localhost ~]# ls
'a file.txt'  'b file.txt'   tori.txt
```
- Word splitting is disabled for characters between those quotes. If the quotes weren't present, two file `a` and `file.txt` would've been created.
# About quotes
Forget everything about quotes you've learnt in other programming languages.
What's the difference between these 3 commands?
```
cat $PWD/*.txt
cat '$PWD/*.txt'
cat "$PWD/*.txt"
```
## No quotes
All available shell expansions are being applied.
## Single quotes
All expansions are disabled, word splitting is disabled.
## Double quotes
Disables most expansions, such as
- tilde expansion (`~`)
- filename expansion (`*`)
- word splitting
But certain expansions are still enabled.
- Variable expansion
- Parameter expansion
![](_resources/Pasted%20image%2020240201120958.png)
Here `PWD` variable is expanded but `file called *` is not expanded.
## What do quotes do?
The quotes only define how bash will expand/split the command. They do nothing else.
![](_resources/Pasted%20image%2020240201121144.png)
Shows all the files. Here both variable and filenames are expanded.
![](_resources/Pasted%20image%2020240201121243.png)
It expands nothing. Prints as it is.
# Summary, so far
![](_resources/Pasted%20image%2020240316181536.png)
# Summary, Rules for bashers
- Use double quotes around variables.
- Refer filenames as `./filename.txt`
