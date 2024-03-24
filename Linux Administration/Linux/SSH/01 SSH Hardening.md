# Changing ports
```
vi /etc/ssh/sshd_config
Port 222
```
To check ssh configuration
```
sshd -t
```
Make sure to disable selinux and firewalld and then reboot.
Now, restart sshd.
```
systemctl restart sshd
```
Now try to ssh into this server using this command.
```
ssh root@10.0.2.15 -p 222
```

