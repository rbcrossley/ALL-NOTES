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
