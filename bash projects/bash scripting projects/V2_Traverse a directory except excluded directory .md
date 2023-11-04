The "V1:Traverse a directory except excluded directory in Linux" was flawed.
# Why?
https://mywiki.wooledge.org/BashPitfalls#for_f_in_.24.28ls_.2A.mp3.29

```cmd
#!/bin/bash
given_path=/home/techyman/glassfish4/glassfish/domains
excluded_folder=x
for each in "$given_path"/*; do
    if [ "$excluded_folder" = "$each" ]; then
        echo "This is x"
        echo "$PWD"
        continue
    else
        echo "this is not x"
        echo "$PWD"
        cd $each/logs
        # echo "This is $PWD"
        # echo "This is $x"
    fi
done
```