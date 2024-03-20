```
#!/usr/bin/env bash
# actall.sh
for FN in $*
do
    echo changing $FN
    chmod 0750 $FN
done
```
Output
```
./actall.sh abc.txt another.txt allmynotes.txt
```
Shell will substitute `$*` with all the file names presented in command line parameters.
It's like saying:
```
...
for FN in abc.txt another.txt allmynotes.txt
...
```
* * *
# if filenames consist of blank spaces
```
#!/usr/bin/env bash
# actall.sh
for FN in "$@"
do
    echo changing $FN
    chmod 0750 $FN
done
```
