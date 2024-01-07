# The read command-reading from stdin
```cmd
#!/bin/bash
while (($# > 0)); do
    chars=$(wc -c <$1)
    words=$(wc -w <$1)
    lines=$(wc -l <$1)
    printf "File \'%-12s contains %4d characters, %4d words and %4d lines.\n" \
        $1\' $chars $words $lines
    shift
done
```
Here we process command line parameters. This is one way to get data into our script which we should then process using the positional parameters.
The `$#` tells the script how many command line arguments exists on its command line.
`$#=3` means we pass 3 parameters to the command.
`$1` should be a file name. We tell the shell to open that file and send its content into the standard input of `wc` and save the output as `chars`.
Same thing for `words` and `lines`.
We used here command line arguments and used the positional parameters and the shift command to get the data into our script and process it. Then, here we tell the shell to feed the contents of the first file into the standard input of wordcount(wc). 
Calling shift makes `$2` convert to `$1`.
So, `wc` is a file or program that can read from it's standard input and we actually want to do the same.
The command used for that purpose is `read`. `read` reads from standard input.
Standard input is usually connected to keyboard. So, we can type on keyboard and feed data to our script that way.
We can also use the pipe character and pipe the data into the standard input channel and we can process the data that way in the script.
`read` command is built into the shell. It's not a standalone command and you won't find any explanation about the shell's read command in the manual pages.
To learn either go to `man bash` and read about `read` or read the bash reference manual.
Once I type `read` and hit enter, the shell will invoke the command and `read` will be in charge.
What does `read` command do when it's in charge?
I can type here on the keyboard and `read` echoes the character I type to the screen.
The shell isn't involved. The shell just set off the command and is now waiting for it to end. And then it'll check the exit status.
The manual pages means `read` actually processes data in chunks of 1 line at a time.
To process these data, you probably know what comes next. We have to have these data available in a variable and be able to access that variable using a variable name.
And we tell read to do that, save what comes from the keyboard, the line in a variable and call it LINE.
I could use any word here. But LINE seems to make  a lot of sense here at the moment.
```cmd
read LINE
hit enter
```
`read` is in control and I type something else here on the screen, using keyboard and hit enter and again read exits, terminates but my data are now available through the variable name LINE.
```cmd
echo $LINE
output
```
Here, I just echo to the screen. But I can do anything I want with it now.
So, the scripts that we're going to write later, they'll do whatever they have to do using this mechanism to get the data into the script and access it through variable name, I can actually specify more than 1 variable.
```cmd
read var1 var2 var3
enter
```

```cmd
#!/bin/bash
while read var1 var2 var3; do
    printf "%s\n" "1-$var1" "2-$var2" "3-$var3"
done
```

```cmd
Output
hello friends hello
1-hello
2-friends
3-hello

hello friends
1-hello
2-friends
3-

hello friends how are you
1-hello
2-friends
3-how are you
```
`read` takes the first word and puts it into the first variable named `var1` then the second into `var2` and the third one into `var3`.
You can see what would happen if I enter less than 3 words or more than 3 words above.
`read` encounters the newline, finds just one word and put it into the first variable and the other two variables are empty.
What if I enter more words than that?
You can see above.
`%s` is format string here. It's reused serveral times until all the printf command line arguments are processed.
So, if I don't want read to echo the characters while I am typing I can use the `-s` option. `s` stands for silent.
You won't see what you're typing if you turn on this option.
```cmd
#!/bin/bash
while read -s var1 var2 var3; do
    printf "%s\n" "1-$var1" "2-$var2" "3-$var3"
done
```
Another option that I can specify is a prompt.
```cmd
#!/bin/bash
while read -p "Type Something" var1 var2 var3; do
    printf "%s\n" "1-$var1" "2-$var2" "3-$var3"
done
```
The prompt calls back because read is called up again and displays a prompt.
And read now waits again.
Another thing I can do is specify timeout using a option `-t` and a numerical value like 5. `read` will wait 5 secs for me to hit enter and then timeout if I don't hit enter within that timestamp. i.e read will terminate after waiting for 5 secs of inactivity.
# Some math in filter
```cmd
#!/bin/bash
while read -p "Type Something" -t 5 var1 var2 var3; do
    printf "%s\n" "1-$var1" "2-$var2" "3-$var3"
done
```
We can write scripts that function as filters. That means they read from standard input and write to standard output like for eg `wc` or `grep`. There are many filters. We encountered quite a few of them already.
How'd it look in a script if I use `read` to write an actual filter.
The last script is already a filter because it reads from standard input and it prints to the standard output.
That's all the qualification of a filter.
We need a file with lots of lines or anything, any program that outputs a lot of lines that we can feed then into standard input of testread(above script name). So, there is man bash banual page that has quite a few lines. So, we can use those lines to test our testread.
```cmd
man bash | bash testread
```
It reads every single line then took the first word in the line into variable 1, the second line into variable 2 and all the rest went to the third remaining variable.
`read` didn't prompt because date was already there. So, read didn't need to wait, didn't need to prompt and also the timeout was not an issue.
But now, let's write something more useful and one idea that came- I want to write a utility, a filter that computes a sequence of numbers.
So, if I give it a number, I want it to consider that number the end of a sequence of natural numbers starting at 1.
So, if I give it a 5, I wanted to compute
1+2+3+4+5=15 and then I want to output 15 and also the original number 5 and a space in between.
```cmd
sums
#!/bin/bash
while read number; do
    i=$number
    sum=0
    while ((i > 0)); do
        ((sum += i--))
    done
    printf "%5d %-d\n" $number $sum
done
```

```cmd
%d integer
%5d field width=5
%-d left justified integer
\n new line
bash sums
```
```cmd
# bash sums
10
   10 55
```

```cmd
echo 10| bash sums
10 55
echo 100| bash sums
```
I can't do this. I can't just echo them this way, because this is just 1 line. So, `read` will put the entire line into a variable and that is not going to end well. At least we will not get any  useful results and error messages.
So, we've to put these numbers into a file and one number per line because `read` reads lines.
So, let's make a file called numbers and put the numbers there like this.
```cmd
5
10
12
43543
45
767
345
```
This is a hard way to do it. Be careful with large numbers though.
```cmd
cat numbers | bash sums
```
This is a hard way. There's a smart way doing this computation that works without that inner while-loop and you probably know what that is.
```cmd
#!/bin/bash
while read number; do
    ((sum = (number * number + number) / 2))
    printf "%5d %-d\n" $number $sum
done
```
It's best way as it takes less time to do calculations compared to the above script.
# Some file processing in a filter
I want to write a script to do same as `wc` but that takes multiple filenames as input.
Jobs in the script.
- First find out the size of the file "filename", use `stat` utility for it.
- Specify what you exactly want by `-c` option, followed by `%s` that tells `stat` to return the size of the file whose name I pass it. This call to `stat` will return a number.
- We use (()) because we're dealing with arithmetic expressions.
- bytecount=grand total of the file sizes
- filecount=count the files
- Then we just have to print everything on screen.
```cmd
bytecount.sh
#!/bin/bash
bytecount=0
filecount=0
while read FILENAME; do
    ((bytecount += $(stat -c%s "$FILENAME")))
    ((filecount++))
done
printf "%d %d\n" $filecount $bytecount
```
This takes files as input.
```cmd
echo bytecount | ./bytecount.sh
1 174

ls| ./bytecount
13 5502

find ~/bash/talks | ./bytecount.sh
```

Filenames with backslashes and filenames with new lines? 

```cmd
find /etc | ./bytecount.sh
find /etc | wc -l
```
# Read's exit status
```cmd
#!/bin/bash
while read -p "- " LINE; do
    echo -$?
    echo --$LINE
    echo --$?
done
echo ---$?
```
We actually can't look directly at read's exit status when it fails-not within a while loop.
# read and pipelines
`read` reads from  standard input. 
```cmd
echo hello|read LINE;echo $LINE

LINE=tuesday
echo $LINE
tuesday

echo hello|read LINE;echo $LINE
tuesday
```
It's as if `read` has done nothing. But interesting thing is this is legal. And this is going to give you a frustrating lead.
> Each command in a pipeline is executed in its own subshell, which is a separate process

So, when the shell encounters the pipe symbol, it creates a subshell. And in that subshell, it starts the `read` command and then it sends the output of this `echo` call to the standard input of the `read` command in other subshell and `read` indeed reads the input and assigns it to a variable named `LINE`, but then it terminates with `read` terminating the entire subshell terminates and disappears and so does the variable named `LINE`.
So, `LINE` still would have the same value as before.
Actually, it's a different variable. We just named it the same. So, `echo $LINE` is what we saw before. It's still Tuesday even though you might think it should be hello.
So, that's the reason. This read call happened in different process, in a different shell and it's gone when we came to this `echo` call.
So, there are few ways how you can take care of this problem, how you can make it work the way it looks.
One thing is, you can enclose these 2 commands in parentheses. Parentheses force the creation of subshell.
All the commands within the parameters are executed in that subshell.

```cmd
echo hello | (
    read LINE
    echo $LINE
)
hello
echo $LINE
tuesday
```
Since the process was gone `echo $LINE` is still tuesday.

Another way to deal with this is to use curly braces.
```cmd
# echo hello| { read LINE ; echo $LINE; }
hello
```
Third way to handle this is to use script which is what we're going to do a lot in this course.
```cmd
#test
#!/bin/bash
read LINE
echo $LINE
```

```cmd
echo friday|bash test
friday
```
# read and backslashes (read -r)
As you know by now, some characters are special to the shell.
If I try to echo the metacharacters to the screen, print them to the screen, that will not end well.
Because these characters are special to the shell.
So, shell will try to perform some actions when it encounters any one of them.
Thus put special characters i.e metacharacters inside quotes(single or double). Alternatively, I can use backslashes, use backslash before every special characters. Good to use this when there is only 1 such character.

The backslash character removes the special meaning of the character immediately following it. What would LINE variable actually do with these metacharacters?
```cmd
read LINE;echo "$LINE"
```
All the characters displayed on screen without modification without any error. Even though I didn't use quotes or backslash, read actually considers these characters just ordinary characters without any special meaning.
Actually, I could not have used quotes, because also the quotes have no special meaning. So, an odd number of quote would create the problem with the shell but not with 3 because it's just a quote without special meaning, just the quoting character.
But space, tab and newline still have a special meaning also with read.
space separates words in read commands.
```cmd
hello        friends
1-hello
2-friends
3-
```
tabs were word separators, read removed them and hello was the first word, goes into variable1, remaining 1 is empty.
I may want just a space, not a word separator.
If I put backslashes before normal characters, backslashes are ignored.

Backslash immediately followed by newline continues the input on the next line.

Backslashes are legal within filenames.
```cmd
find / | grep '\\'
```
Here find produces all file names in the system.
`\\` lists all files with backslashes on them.
Within grep backslash has a special meaning. So, I have to escape that special meaning even for grep.
**Even within single quotes.**
So, that both backslashes will reach the grep and the first backslash will tell grep to actually look for filenames with a backslash in it and not use the backslash in any other way.
```cmd
find / | grep '\\' | ./readls
```
The `readls` script looks like this:
```cmd
#!/bin/bash
while read LINE; do
	ls -ld "$LINE"
```
The output will be nothing in the former one-liner, because you're finding filenames with backslash in it but read ignores the backslash, so it gets nothing.
Sometimes I want read to read backslash as backslash.
Like, if I process the contents of a text file without backslashes in it. For example, the manual page of bash contains a lot of backslashes. So, if I just read that file line by line using read and then print it to the screen, all the backslashes will be removed.
That means, read will actually modify the input that I want to process and we want to prevent that sometimes.
So, in that case we've to tell read to leave backslash alone and we do that using `-r` option.
If this option is given, backslash doesn't act as an escape character. Now, backslash is considered to be the part of LINE.
Modify the above script to this.
```cmd
while read -r LINE; do
	ls -ld "$LINE"
```
## -d option
By default newline is the delimiter. Newline terminates one call to read and the -d option here is used to change that.
```cmd
read -d x LINE
```
It'll make x as a line delimiter. Then as usual, it'd read the input into a variable called LINE.
```cmd
while read -p "Type something:" -d x var1 var2 var3; do
    printf "%s\n" "1-$var1" "2-$var2" "3-$var3"
done
``` 
read terminates once it encounters x.
 
 We solved a few problems with the special characters in filenames before. we had spaces in filenames and we solved that using quotes(double quotes or single quotes). We had backslashes in filenames and we solved that using -r option. -r leaves a backlash alone. And when backslash is a part of filename then we want to be that way. Otherwise read would remove the backslash. So, this -r will remain in place and that's why we can process the filename and the ls call is just one example how to process file names.
 But actually any character on the keyboard can end up in a filename.
 To solve the problem of piping files with newlines, we've to tell read here in readls, for example to treat the newline as an ordinary character.
 i.e use -d option(50% of solution).
 With -d I can change the line delimiter from newline to another character.
 Question is which character?
 We used x before but x can be a part of filename.
 Even non-printable characters which you'll not be able to enter from keyboard, they can be part of a filename.
 So, just about anything can be part of a filename, so which character do we use?
 Well, in Linux there's only 1 character that can't be part of filename and it's null byte. That's a byte where all bits are zero. There's no way you'll ever get 0 in a filename. Kernel won't allow it. So use null byte as a line delimiter because it'll never be part of the name itself.
 How to specify it?
 ```cmd
 #readls
 while read -rd '' LINE; do
    ls -ld "$LINE"
done
```
We'll not see anything useful here because now read waits for the null-byte as a line delimiter and there's none. So, we don't get any output. So, we have to find a way now to get the listing into the script, but each line should be delimited by a null-byte rather than a newline. So, ls can't do that but many other utilities can.
If I use find on current directory. find lists all the files now in the current directory and in the sub-directories, but I can tell find to terminate the line not by newlines but by null-bytes.
```cmd
find . -print0<enter>
```
It looks messy. If you look closely, the reason is that we've null bytes between the file names and they've no representation on the screen.
The newline is preserved in filenames with newlines because new line isn't a line delimiter. That's other half of solution.
```cmd
find . -print0| bash readls
```
It's wonderful, my files with newlines are getting printed correctly.


