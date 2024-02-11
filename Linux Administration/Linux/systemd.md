References:
https://www.udemy.com/course/mastering-linux/
Unix and Linux System Administration Handbook by Evi Nemeth.

systemd is a process that manages OS.
It's not a part of kernel.
It doesn't talk to hardware directly.
It's PID=1.
```
[root@node ~]# ps 1
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:09 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
```
This process will initiate all the rest. It'll start the system along with all the services that we need to run our system.
Maybe we need a service to manage our network card to connect to wifi or we need a service to synchronize the time, or service to mount drives.
It can be used to start services like website.
![](_resources/Pasted%20image%2020240107230613.png)
systemd isn't just PID=1, there's lots of systemd like journald, udevd, logind etc.
## Why systemd?
- we want a webserver to be launched in the background.
- we want systemd to execute a command on every boot.
- we want to have a command to be run every few minutes, no matter if we're logged in or not.
## Two modes
- system mode
- user mode
### System mode
- read unit files
- build dependency graph
- executes all commands necessary to get to the result
### User mode
- same as system mode but for user services
# Unit
- basic building blok
- various types available
	- service
	- target
	- timer
	- ....
- units can depend on another.
## Service
- special type of unit
- represents a service that should run on our system.
- service can be enabled, disabled, started, stopped, restarted, reloaded.
- service could be a webserver.
- service could be a script that should be run every few minutes.
Anything systemd can launch is a service. Service is still a unit. A special kind of unit(building block).
If file extension is .service, then it's service unit file.
### Where do we find service files?
- /lib/systemd/system
- /usr/lib/systemd/system
- /run/systemd/system
- /etc/systemd/system
Files in /etc have the highest priority.
We can find those paths with the help of following CLI commands.
```
systemd-analyze --system unit-paths
```
# Manage a unit
**Get the status of a unit:**
``systemctl status unit``
**Change the status of a unit:**
```
systemctl {start,stop,restart,reload} unit
```
reload asks the unit to reload its configuration.
This doesn't reload the systemd unit file, it only asks the service to reload its configuration.
# systemd logging
systemd messages captured by journald are stored in the /run directory. rsyslog can process these messages and store them in traditional log files or forward them to a remote syslog server.
You can configure journald to retain messages from prior boots. To do this, edit /etc/systemd/journald.conf and configure the Storage attribute.
```
[Journal]
Storage=persistent
```
You can now get prior boots with
```
journalctl --list-boots
```
## To restrict the logs associated with a specific unit, use the `-u` flag:
```
journalctl -u ntp
```
## To find the units
```
systemctl list-units --type=service
```
# cgroups
They are mechanism to organize process groups. cgroup can span multiple processes and even contain other cgroups.
## Use of cgroups
- It helps to limit resources. We can configure a cgroup to not exceed a specific memory limit. We can also define how much % of CPU resources this cgroup may occupy.
- It helps to measure how much resources certain systems consume.
- It helps to freeze a group or processes.
## Inspect cgroups
To show processes on our system
```
systemctl status
```
## To inspect cgroups
```
systemd-cgtop
```
By default, it displays upto 3 levels of cgroups.
We can change this level to `--depth=5`.
### To find the path of executable
```
which firefox
```
![](_resources/Pasted%20image%2020240114221138.png)
# There is a process already using a port glassfish error
https://stackoverflow.com/questions/44654024/glassfish-there-is-a-process-already-using-the-admin-port-4848
Although there is no running Java process and the port is not occupied, you hostname is probably not pingable:
```
ping $(hostname)
```
output: Request timeout
Solution:
Add the following line to the `/etc/hosts` file:
```
127.0.0.1 [YOUR_NOT_PINGABLE_HOSTNAME]
```
OR
`pkill -f glassfish`
OR
Be sure to delete the `$PATH/TO/domain1/config/pid` and `$PATH/TO/domain1/config/pid.prev` files as well, if the process isn't running but is being reported as still running.
* * *
# Practical about systemd memory limiting
Our Objective is to limit glassfish to only consume 100 MB of memory. 
## Unit file for glassfish
```
[Unit]
Description = GlassFish Server v4.1
After = syslog.target network.target

[Service]
User=techyman
Group=techyman
ExecStart = /home/techyman/.java/bin/java -jar /home/techyman/glassfish4/glassfish/lib/client/appserver-cli.jar start-domain
ExecStop = /home/techyman/.java/bin/java -jar /home/techyman/glassfish4/glassfish/lib/client/appserver-cli.jar stop-domain
ExecReload = /home/techyman/.java/bin/java -jar /home/techyman/glassfish4/glassfish/lib/client/appserver-cli.jar restart-domain
Type = forking

[Install]
WantedBy = multi-user.target
```
## Command to limit systemd glassfish to 100MB
```
systemd-run --scope -p MemoryHigh=100M --user  /home/techyman/.java/bin/java -jar /home/techyman/glassfish4/glassfish/lib/client/appserver-cli.jar start-domain domain1
```
***
# systemd targets
![](_resources/Pasted%20image%2020240125173945.png)
# Targets
```
[root@w2 ~]# systemctl cat nginx.service
# /usr/lib/systemd/system/nginx.service
[Unit]
Description=The nginx HTTP and reverse proxy server
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
# Nginx will fail to start if /run/nginx.pid already exists but has the wrong
# SELinux context. This might happen when running `nginx -t` from the cmdline.
# https://bugzilla.redhat.com/show_bug.cgi?id=1268621
ExecStartPre=/usr/bin/rm -f /run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
KillSignal=SIGQUIT
TimeoutStopSec=5
KillMode=mixed
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
Read this as 
>nginx.service is wanted by multi-user.target unit when this unit is enabled.

The only targets to be really aware of are `multi-user.target` and `graphical.target` for day to day use. And `rescue.target` for accessing single-user mode.
![](_resources/Pasted%20image%2020240126091740.png)
![](_resources/Pasted%20image%2020240126092813.png)
![](_resources/Pasted%20image%2020240128084856.png)
![](_resources/Pasted%20image%2020240128084916.png)
# systemd for own program
## About time
The current time was about 09:xx AM.
```
[root@master ~]# date +%H
09(HOUR)
[root@master ~]# date +%I
09(I=MIN)
[root@master ~]# date +%I%P
09am(P means AM/PM)
[root@master ~]# date +%I%P:%M
09am:08(M means minutes)
[root@master ~]# date +%I:%M%P
09:10am
[root@master ~]# date +%T
09:12:08(T means full time HH:MM:SS)
[root@master ~]# date +%m/%d/%Y
01/28/2024(m means month, d means day, Y means year)
```
## Unit file
```
[Unit]
Description=Ping a server and log it
Requires=network-online.target
After=network-online.target

[Service]
Type=oneshot
StandardOutput=append:/network-log/log.txt
ExecStart=date '+%%T'
ExecStart=ping -c 4 google.com

[Install]
WantedBy=multi-user.target
```
Read this as only after "`network-online.target`" has been fully started, start this unit file.
If output is generated, append it to `/network-log/log.txt`.
Put this file in `/etc/systemd/system`.



