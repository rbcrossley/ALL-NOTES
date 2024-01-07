You can't do arithmetic in shell scripting normally. Because every variable in shell by birth is a string.
```cmd
# k=7
# k=k+10
# echo $k
k+10
```

Putting `$` won't work either to do arithmetic.
```cmd
# k=7
# k=$k+10
# echo $k
7+10
```

# let
let helps do shell arithmetic.
```cmd
# let 'k=k+10'
# echo $k
17
# let 'k=k+10'
# echo $k
27
```
let can take multiple parameters as well.
```cmd
# let 'k=k+10' 'result=k*3'
# echo $k
37
# echo $result
111
```
You can put variables inside double quotes. Putting into double quotes, shell will interpret the sign `$` as variable.
```cmd
# let "result=$result*3"
# echo $result
333
# let "result=$result*3" 'num=0'
# echo $?
1
# let "result=$result*3" 'num=1'
# echo $?
0
```
Above code makes it clear that the exit status depends on the result of last passed argument in `let`.
Meaning, if the result of last passed parameter is 0, then exit status is FAILURE i.e greater than 1.
Else, if the result of last passed parameter is 1, the exit status is SUCCESS, i.e 0.

# declare
```cmd
# declare -i k
# echo $k
37
# k=k+10
# echo $k
47
```
`declare -i` means to declare `k` as integer. Then it will be treated like one.

# expr
```cmd
# expr 3+4
3+4
# expr 3 + 4
7
# expr 3 > 4
# ls
4  
# expr 3 \> 4
0
```
Put spaces between operators and operands while using `expr`. You've to escape the `>` operator while using it as `greater than` operator for checking.
Else, new file will be created named whatever is there in Right Hand Side.
The output of using comparison operator in this case is not like what happens in exit status. Here, if the result is true, output will be 1 else 0.

# (( ))
Whenever the shell sees the open parentheses, it'll go to arithmetic mode. 
```cmd
# (( num=100 ))
# echo $num
100
# ((num++))
# echo $num
101
```
Everything in arithmetic is either variable, value or operator and commonly it is an operator.
## Comma operator
```cmd
# (( k=10 , j=20, result=j*k ))
# echo $k $j $result
10 20 200
```

## String converted to integer
has value of 0.
```cmd
# j=Paris
# echo $j
Paris
# (( k=j+5 ))
# echo $k
5
# echo $j
Paris
```
## Comparison operator
No need to escape like in expr when `(( ))` is used.
```cmd
# echo $((3 < 4))
1
# echo $((3 >  4))
0
```
The exit status for 1(true) is 0 and for 0(false) is greater than or equals to 1.

# Exit Status of Arithmetic Expressions
Each arithmetic expression has
- arithmetic value
- exit status
They work like mentioned above.