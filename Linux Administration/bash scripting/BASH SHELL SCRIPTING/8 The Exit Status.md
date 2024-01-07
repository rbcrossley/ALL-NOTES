# The Exit status and how to retrive it: `$?`
Terminates and give an exit status.
`$?` success=0

`$?` failure=1-255

When pipe is used, the exit status of last command is considered.

```cmd
echo $?
1-255
echo $?
0
```
Never check echo exit status twice.

`true` command, does nothing and succeeds. All it does is set exit-status=0.

`false` command does nothing but fails. All it does it set exit-status>0.

# AND and OR combination
```cmd
if this is true
	echo true
else
	echo false
```
equivalent to
`true && echo true || echo false`

To close the error channel
`true && echo true 2>&- || echo false`

`ls file 2>&- >/dev/null && rm file || touch file`
Closes the error channel and sends the output to `/dev/null`.
This is really inefficient way to check for the existence of the file. Better way is to use the test command.

`[ -f file ] && rm file || touch file`

`touch file`

`[ -f file ]`

Since file exists, the exit status will be 0.

`echo $?`

0

`rm file`

`[-f file]`

`echo $?`
Since file doesn't exist, the exit status is 1 or anything greater than 0.
`1`
