# nginx
```
---
- name: set up webserver
  hosts: localhost
  tasks:
    - name: ensure nginx is at the latest version
      dnf:
        name: nginx
        state: latest
    - name: start nginx
      service:
        name: nginx
        state: started
        enabled: yes  # if you want to also enable nginx

```
# docker
```
---
- name: Install Docker on Rocky Linux 9
  hosts: localhost
  become: true

  roles:
    - name: geerlingguy.docker
      vars:
        docker_packages:
          - docker-ce
          - docker-ce-cli
          - containerd.io
```

```
ansible-galaxy install geerlingguy.docker
ansible-playbook docker-rocky.yml
```
## Manually install docker
```
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install -y docker-ce
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```
# Glassfish
```
sudo dnf install java-11-openjdk
java -version
sudo dnf install unzip
sudo useradd -m -d /opt/glassfish6 -U -s /bin/false glassfish
cd /tmp
wget https://download.eclipse.org/ee4j/glassfish/glassfish-6.2.5.zip
unzip /tmp/glassfish-6.2.5.zip -d /opt
sudo chown -R glassfish:glassfish /opt/glassfish6
sudo nano /lib/systemd/system/glassfish.service
```

```
[Unit]
Description = GlassFish Server v6
After = syslog.target network.target

[Service]
User=glassfish
ExecStart=/opt/glassfish6/bin/asadmin start-domain
ExecReload=/opt/glassfish6/bin/asadmin restart-domain
ExecStop=/opt/glassfish6/bin/asadmin stop-domain
Type = forking

[Install]
WantedBy = multi-user.target
```

```
sudo systemctl daemon-reload
sudo systemctl start glassfish
sudo systemctl enable glassfish
sudo systemctl status glassfish
```