```cmd
grep -v "bash"
```
everything that doesn't contain bash.

```cmd
cat bash_manual.txt | grep "file <newline
>version
>input"
```

Every line that contains file, version and input.

```cmd
grep "version" tee_manual.txt top_manual.txt
```
Check for version in both the files.

```cmd
grep -n "version" tee_manual.txt top_manual.txt
```
Check for version in both the files.

```cmd
grep -n -f patterns tee_manual.txt top_manual.txt
```
patterns is a file containing patterns. Each pattern should be in a new line.

# More about grep
`grep -no "above" test.txt`
`n` to give line numbers and `o` to omit everything except the string to match i.e above.
`grep -c  "above" test.txt`
How many lines(count), above is there in that file?
 
## Very Important ##
`-A` To display N lines after match.
`grep -A 3 "string" file`

`-B` To display N lines before match.

`-C` To display N lines around match.

`-w` To match a whole word.

`-r` To  search recursively under this directory. To search under current directory and its sub-directory.

`-l` To display only file names.

`-h` To hide file names. To only print the content.


# Advanced grep command
`grep -e "line" -e "above" -e "bash" -e "shell" test.txt`
As it suggests, this is to find either of these words in a file.
The simplified version of this is
`grep -E "line|above|bash|shell" test.txt`
Pattern means more than 1 string.


## Rules to create pattern
`xy|pq`
Matches for xy or pq.
`^xyz`
Matches for line starting with xyz.
`xyz$`
Matches for lines ending with xyz.
`^$`
Matches for lines which are empty.
`\`
To remove special purpose of any symbol.
`grep -E "\^" test.txt`
Print a line which are having `^` symbol.
Matches any one character.
`grep -E "." test.txt`
Any means all characters will match.
Example, Strings starting with t, 2 characters and then s.
`grep -E "t..s" test.txt`
`\b` matches empty string at the edge of the word.
`grep -E "line\b" test.txt`
Spaces at the start and end.
`grep -E "\bline\b" test.txt`
Exact word matching.
`grep -Ew "line" test.txt`
`?`
Preceding character is optional, will be matched at most once.
`grep -E  "yf" test.txt`
If f is optional, you do this. It means max 1 time match.
`grep -E "yf?" test.txt`

`*` means that the preceding character willl be matched 0 or more times.

`grep -E "yf*" test.txt`
`*` is applicable for f only. f may be 1 or more times.
`+` minimum 1 times, maximum may be any number of times.

`grep -E "yf+" test.txt`

`[xyz]` matches for the lines which are having x or y or z.

`grep -E "t|T|f|y" test.txt`
is equivalent to
`grep -E "[tTfy]" test.txt`

`[a-d`
characters either a or b or c or d.
You could also use pipeline for it.
`grep -E "a|b|c|d|e|f" test.txt`
`grep -E "[abcdef]" test.txt`
`grep -E "[a-f]" test.txt`

```cmd
grep -E "a|b|c|d|p|q|r" test.txt
grep -E "[abcdpqr]" test.txt
grep -E "[a-dp-r]" test.txt
```

`^[abc]`

Matches for lines starting with either a or b or c.

Matches either x or y matching line
`grep -E "[xy]" test.txt`
Matches line starting with x or y.
`grep -E "^[xy]" test.txt`
But
`grep -E "[^xy]" test.txt`
looks for other than x or y.

Curly braces
`grep -E "xfff" test.txt`
`grep -E "xf{4}" test.txt`

Other stuffs
```cmd
[[:digit:]]
[[:upper:]]
[[:blank:]]
[[:alnum:]]
```