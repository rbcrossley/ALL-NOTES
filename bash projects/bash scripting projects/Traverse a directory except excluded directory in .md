# Script
```cmd
#!/bin/bash
given_path=/home/techyman/glassfish4/glassfish/domains
excluded_folder=x
for each in $(ls $given_path); do
    if [ "$excluded_folder" = "$each" ]; then
        echo "This is x"
        continue
    else
        echo "this is not x"
        cd $given_path/$each/logs
        echo "This is $PWD"
    fi
done
```
# What it does
- This goes to given path.
- Does a listing by  ls
- If that ls equals to excluded folder, it'll skip it with that continue statement
- Else it goes inside logs directory and does some actions.
