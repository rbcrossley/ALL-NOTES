Disclaimer
- I didn't create these commands. Thanks to my seniors, unix.stackexchange.com for all these commands.
- They're not in particular order.
- I'm a newbie so take these commands with a grain of salt.
# Find if a string exists within proximity between another string
Say you know a string1 as a placeholder in your logs and you want to pick an error(string2) near that phone number.
```
what_we_want='error'
context='placeholder'
distance=10
grep -E -e "${context}" -C "${distance}" file_to_look_into | grep -E -e "${what_we_want}" -C "${distance}"
```
# Sort by first column name
You want to sort the output of disk usage command by the size.
```
du -sc *| sort -s -n -k 1,1
```
# Transfer files from windows to linux VM
`sftp://user@ipaddress:port`
No need to insert port if port 22 is being used.
Example:
`sftp://root@10.20.30.11`
# Give execute permissions to every files in a directory
`chmod -R 777 .`
# Find logs between two time durations
This is by far the most used command in my day to day life.
```
LC_ALL=C awk -v beg=12:15:00 -v end=13:00:00 '
  match($0, /[0-2][0-9]:[0-5][0-9]:[0-5][0-9]/) {
    t = substr($0, RSTART, 8)
    if (t >= end) selected = 0
    else if (t >= beg) selected = 1
  }
  selected' name_of_log_file
```
This finds the logs between two time durations where the time is in form of `HH:MM:SS`
Thanks to chatgpt for more advanced version of this command which is more easier.
```
#!/bin/bash

# Check if three arguments are provided
if [ "$#" -ne 3 ]; then
    echo "Usage: $0 <beg_time> <end_time> <log_file>"
    exit 1
fi

beg=$1
end=$2
log_file=$3

LC_ALL=C awk -v beg="$beg" -v end="$end" '
  match($0, /[0-2][0-9]:[0-5][0-9]:[0-5][0-9]/) {
    t = substr($0, RSTART, 8)
    if (t >= end) selected = 0
    else if (t >= beg) selected = 1
  }
  selected' "$log_file"
```
# Empty a file
```
echo > catalina.out
OR
> catalina.out
```
Both will do the same thing.
# Copying multiple files to take backup using cp command
If all the files are having same extensions:
```
cp *.conf nginx_conf_bak
```
If the files don't have same extensions:
```
cp file.conf file2.conf file3.conf backuppedconf
```
backuppedconf will contain all three files.
# Search logs of multiple months
`grep -lw  -e 'string_to_search' name_of_log_2023-0[7-9]-*`

# split a large file in smaller pieces and store them in /tmp

```
split -n 20 name_of_log.log /tmp/smallfile
```
This will split the file named `name_of_log.log` into 20 different pieces and save them into `/tmp/smallfile`  as `smallfileaa smallfileab ...`
# Recursively search search for files containing a string that are .gz files
```cmd
find . -mtime -15 -name "*.gz" -exec zgrep -lH "string" {} \;
```
 `-` means younger than 15 and `+` will mean older than 15.

# Recursively search for text files
`grep -rlw . -e 'string_to_search'`

# Save the output of earlier tailed logs to a file?

```
tail -f application.log | tee -a /tmp/log
tee will save the STDIN to a file and also send it to STDOUT. (without -a it will overwrite any existing file).
```
This is yet another most used command in my day to day life to capture real time logs.

# To insert new line after new paragraph after date in logs
```
:%s/\d\{4}-\d\{2}-\d\{2} \d\{2}:\d\{2}:\d\{2}/\r&/
OR
:%s/\d\d\d\d-\d\d-\d\d \d\d:\d\d:\d\d/\r&/
```
The date format being
```
2023-11-26 14:14:14
```

To do this without using vi editor, use this command
```
sed -E 's/\b([0-9]{4}-[0-9]{2}-[0-9]{2} [0-9]{2}:[0-9]{2}:[0-9]{2})\b/\1\n/g' inputfile > outputfile
```
# SSH Tunneling
![](_resources/Pasted%20image%2020231226171157.png)

```
ssh -p <SSH port> -L <Forwarded port>:<Remote server>:<Remote port> <SSH login>@<SSH server>
```
![](_resources/Pasted%20image%2020231226171317.png)


# To apply permissions for files with same extension at once
```
find /images/. -name "*.jpg" -exec chmod 0644 {} \;
```
# To highlight multiple strings in less editor
```
/foo|bar
```
# Search exact match word in vi editor
```
/\<FOO\>
```
# Case insensitive search in less/vi editor
Just add `\c` at the end.
```
/copyright\c
```
