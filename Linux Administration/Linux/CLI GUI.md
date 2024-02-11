# select
It asks user to select any of the given provided variables and saves it in a variable called product. Later it's echoed by accessing it using `${}` sign.
```
select product in "Orange" "Apple" "Pineapple" "Mango"; do
    echo "${product}"
done
```
To terminate it, after selecting an item:
```
select product in "Orange" "Apple" "Pineapple" "Mango"; do
    echo "${product}"
    break
done
```
To print what the user selected, use the variable `REPLY`.
```
select product in "Orange" "Apple" "Pineapple" "Mango"; do
    echo "${REPLY}: ${product}"
    break
done
```
Output
```
[root@localhost ~]# bash select.sh
1) Orange
2) Apple
3) Pineapple
4) Mango
#? 1
1: Orange
```
- pineapple etc would work without quotes if there was no space in between.
## Configuring the select
To configure the select, we can change the following variable:
`PS3`
```
PS3='Please choose your product: '
select product in "Orange" "Apple" "Pineapple" "Mango"; do
    echo "${REPLY}: ${product}"
    break
done
```
Output
```
[root@localhost ~]# bash select.sh
1) Orange
2) Apple
3) Pineapple
4) Mango
Please choose your product: 1
1: Orange
```
# option
```
select option in "uptime" "user" "free diskspace" "quit"; do
    echo "${option}"
    break
done
```
Output
```
[root@localhost ~]# bash option.sh
1) uptime
2) user
3) free diskspace
4) quit
#? 1
uptime
```
We want to do things based on the chose options, thus:
```
select option in "uptime" "user" "free diskspace" "quit"; do
    case "${option}" in
    uptime) uptime ;;
    user) echo "${USER}" ;;
    "free diskspace") df ;;
    quit) break ;;
    *) echo "option not found" >&2 ;;
    esac
done
```
Output
```
[root@localhost ~]# bash option.sh
1) uptime
2) user
3) free diskspace
4) quit
#? 1
 17:24:19 up 20:16,  1 user,  load average: 0.00, 0.00, 0.00
#? 2
root
#? 3
Filesystem          1K-blocks    Used Available Use% Mounted on
devtmpfs                 4096       0      4096   0% /dev
tmpfs                  903156       0    903156   0% /dev/shm
tmpfs                  361264    6452    354812   2% /run
/dev/mapper/rl-root  17756160 2112128  15644032  12% /
/dev/sda1              983040  227828    755212  24% /boot
tmpfs                  180628       0    180628   0% /run/user/0
#? 4
```
