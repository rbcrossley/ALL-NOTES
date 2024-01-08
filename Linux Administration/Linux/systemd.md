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