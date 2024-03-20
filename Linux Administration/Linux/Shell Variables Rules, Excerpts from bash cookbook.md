# Spaces between name=value not allowed
```
#!/bin/bash
MYVAR = "something"
```
Output:
There can't be space between name, value pairs.
```
$ shellcheck myscript
 
Line 2:
MYVAR = "something"
      ^-- SC2283 (error): Remove spaces around = to assign (or use [ ] to compare, or quote '=' if literal).

$
```
