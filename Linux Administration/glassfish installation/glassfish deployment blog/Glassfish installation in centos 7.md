# Add User
```cmd
useradd techyman
passwd techyman
*enter password*
```
# Install java
```cmd
Get .tar.gz jdk 8 version from oracle website
su - techyman
Download jdk to ~ of techyman
tar -zxvf jdk1.8.0_77.tar.gz
mv jdk1.8.0_77 .java
```
# Bash profile changes
```cmd
You are still in techyman
vi .bash_profile

# User specific environment and startup programs

PATH=$PATH:$HOME/.local/bin:$HOME/bin
export JAVA_HOME=/home/techyman/.java
export PATH=/home/techyman/.java/bin:$PATH
export PATH
```
## Source the bash profile
```cmd
source ~/.bash_profile
```
# Check java version
```cmd
java -version
You should see 1.8.0_171...
```
# Install glassfish 4.1
```cmd
Download glassfish4.1.zip file from glassfish official website.
```
## Install unzip
```cmd
yum install unzip  -y
```
## Use unzip
```cmd
unzip glassfish4.1.zip
```
Everything, so far is done from `~` of user techyman. 
# Disable firewall and selinux
```cmd
systemctl disable firewalld
vi /etc/selinux/config and convert "enforcing" to "disabled"
```
# Unit service file for glassfish
```cmd
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
# enable, start glassfish
```cmd
systemctl enable glassfish.service
systemctl start glassfish.service
systemctl status glassfish.service
```
# Port 4848 issue even when it's not true
If you've modified your hostname at some point of time, you will get an error called, There is a process already using the admin port 4848 -- it could be another instance of Payara Server or Payara Micro. Command start-domain failed.
Follow the following blog for it.
https://www.adam-bien.com/roller/abien/entry/when_there_is_a_process

# Create domain
```cmd
./asadmin change-admin-password
./asadmin enable-secure-admin
./asadmin create-domain --portbase 5000 a
./asadmin create-domain --portbase 6000 b
./asadmin create-domain --portbase 7000 c
./asadmin create-domain --portbase 8000 d
./asadmin create-domain --portbase 9000 e
```

# Delete domain
```cmd 
./asadmin delete-domain -port 5048 a
```
Port 48, remember.
