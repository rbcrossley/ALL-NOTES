# Find a string on the neighborhood of another string
This scenario is very much useful in logs searching. Say your requirement is that you want to find a log file which contains "string1" and "string2" but within say 10 lines of "string1". This will greatly reduce the time wastage on checking all the logs.
```
find . -exec bash -c 'zgrep -C20 "phone_number" {} | grep -q "payment_type" && echo {}' \;
```
This will output file names.
A faster version of this, why is it faster?(TBD)
```
find . -name "*.gz" -type f -exec bash -c 'for arg; do zgrep -C20 "phone_number" "$arg" | grep -q "payment_type" && echo "$arg"; done' dummy {} +
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
**Thanks to stackexchange for more advanced version of this script. This will collect only the part of file between two time durations, directly from the .log.gz file.**
```
#!/bin/bash

# Check if three arguments are provided
if [ "$#" -ne 3 ]; then
    echo "Usage: $0 <beg_time> <end_time> <log_file_gzipped>"
    exit 1
fi

beg=$1
end=$2
log_file_gzipped=$3

zcat "$log_file" | LC_ALL=C awk -v beg="$beg" -v end="$end" '
  match($0, /[0-2][0-9]:[0-5][0-9]:[0-5][0-9]/) {
    t = substr($0, RSTART, 8)
    if (t >= end) selected = 0
    else if (t >= beg) selected = 1
  }
  selected'
```

# Gzip all logs in a current directory and save them to a different file
```
gzip *
```
# Gzip a directory
```
tar -zcvf archive.tar.gz directory/ 
```
# Unzip a directory
```
tar -zxvf archive.tar.gz
```
# Gzip all logs in a current directory except the one named application.log
```
find . -maxdepth 1 -mindepth 1 ! -name 'application.log'  -exec gzip {} \;
```
This will also gzip to a different file each.
![](_resources/Pasted%20image%2020231231142846.png)
# Empty a file
```
echo > catalina.out
OR
> catalina.out
```
Both will do the same thing.
# Find the biggest single file in the entire directory tree
```
 find . -type f -printf "%s\t%p\n" | sort -n | tail -4
```
This will show the top 4 files that are very big.
```
-n, --numeric-sort
compare according to string numerical value
```
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
sed -e 's/[0-9]\{4\}-[0-9]\{2\}-[0-9]\{2\} [0-9]\{2\}:[0-9]\{2\}:[0-9]\{2\}/\n&/' input > /tmp/output
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
# List only directories in current directory
```
ls -d */
```
# backup all files except some folders
```
rsync --archive --exclude /logs /home/username/gcm-notification/ /home/username/gcm_notification_to_another
```

to exclude multiple directories, do this
```cmd
 rsync --archive --exclude={'logs/','generated*/','osgi*/'} . ~/admin_bak_oct_10_2023
```
Note: Use relative paths with exclude. The paths are relative to the source directory, here I'm probably at the admin directory.

