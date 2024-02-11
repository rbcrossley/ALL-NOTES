Offers a centralized way to install all softwares.
```
dnf upgrade
dnf update
```
Both are same.
`dnf` always keeps local packages up to date.

# Install software
```
dnf install <package>
```
eg:
```
dnf install epel-release
dnf install cowsay
```
# Remove software
```
dnf remove <package>
```
# rpm
archive that contains all the files and configuration required to install a software.
We could download .rpm files manually. As it can easily lead to conflicts.
# Inspect rpm file
```
rpm -qpl file.rpm
```
# Install using rpm file
```
rpm -i a.rpm
```
# Remove rpm file
```
rpm -e zsh
```
# which yum
```
/usr/bin/yum
```
# search for software
```
dnf search <software>
example:
dnf search links
```
# Repositories
We want to install gimp.
```
dnf install gimp
```
How does dnf knows from where to download the software from.
That is what repos are for. We can define repos in the following files.
```
/etc/dnf/dnf.conf
/etc/yum.repos.d/abc.repo
```
# Requirements
centos requires baseos and appstream.
# Dependencies
dnf will handle the resolution of dependencies for us.
Weak dependencies doesn't necessarily required to be installed. They can be either suggested or recommended.
![](_resources/Pasted%20image%2020240208210420.png)
![](_resources/Pasted%20image%2020240208211123.png)
![](_resources/Pasted%20image%2020240208211138.png)
![](_resources/Pasted%20image%2020240208214059.png)
![](_resources/Pasted%20image%2020240208214620.png)
![](_resources/Pasted%20image%2020240208214640.png)
![](_resources/Pasted%20image%2020240208220532.png)
![](_resources/Pasted%20image%2020240208220850.png)
![](_resources/Pasted%20image%2020240208221705.png)
![](_resources/Pasted%20image%2020240208222952.png)
```
dnf upgrade
```
after enabling the module.
![](_resources/Pasted%20image%2020240208223527.png)
![](_resources/Pasted%20image%2020240208223614.png)
