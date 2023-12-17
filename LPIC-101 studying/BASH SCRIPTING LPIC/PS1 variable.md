# PS1 is used to define the default shell prompt for bash.
```
[root@localhost ~]# echo $PS1
[\u@\h \W]\$
```
![](_resources/Pasted%20image%2020231201152745.png)
```
export PS1='\w \$ '
```
Small `w` will show full path to the current directory from home. In below cases, it'll be shown as "`~/a/b/c`"

```
export PS1='\W \$ '
```
Big `W` will show only the current directory, example `~/a/b/c` will be shown as "c" only.
# To show date
```
[root@localhost ~]# export PS1='[\u@\h \d \A] \w \$'
[root@localhost Fri Dec 01 16:09] ~ #
```
`\A` the current time in 24-hour HH:MM format.

![](_resources/Pasted%20image%2020231201161637.png)
![](_resources/Pasted%20image%2020231201161812.png)
# New line
![](_resources/Pasted%20image%2020231201162055.png)
# advanced usage
![](_resources/Pasted%20image%2020231201163037.png)
`\l` means which terminal is it in case of duplication.
`\!` means the history number i.e the number of the current command in the command history.


# References
https://riptutorial.com/bash/example/11476/change-ps1-prompt