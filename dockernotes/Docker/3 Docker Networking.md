- doceker0=bridged network
- Several drivers available in docker
	- bridge
	- host
	- overlay
	- macvlan
	- none

### List docker networks
![](_resources/Pasted%20image%2020231112160135.png)

### Inspect docker network
![](_resources/Pasted%20image%2020231112160241.png)

`docker inspect bridge` and `docker network inspect host` both mean the same thing.

`docker container run -dt --name myhost --network host ubuntu`
ubuntu is the image from which the container is being launched from.
Name of the container is myhost.
The network  used is host network.
The next `docker network inspect host` will show 1 container within the network.
# Understanding bridge networks

![](_resources/Pasted%20image%2020231112161325.png)
C1,C2 and C3 can communicate between each other.
To connect with internet, C1,C2,C3 must first communicate with Docker0 bridge.
```
docker container run -dt --name bridge01 ubuntu
docker container run -dt --name  bridge02 ubuntu
```
Enter inside docker container bridge01
```
docker container exec -it bridge01 bash
apt-get update && apt-get install net-tools
apt-get install iputils-ping
```
Find the ip of bridge02 container
`docker container inspect bridge02`
Ping from inside the docker container bridge01 to bridge02. Ping will be successful.

```
root@bfd22ef2df88:/# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.17.0.1      0.0.0.0         UG    0      0        0 eth0
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth0
```
This was done inside docker container bridge01. And it shows that for destination 0.0.0.0 i.e the internet, the gateway i.e the router is 172.17.0.1.
# User defined bridge
- User defined bridges provide better isolation and interoperability between containerized applications.
- **User-defined bridges provide automatic DNS resolution between containers.**
- Containers can be attached and detached from user-defined networks on the fly.
- Each user-defined network creates a configurable bridge.
- Linked containers on the default bridge network share environments.
**Create a bridge network driver.**
`docker network create --driver bridge mybridge`
Create a docker container that is named mybridge01, that uses network mybridge and image ubuntu.
 `docker container run -dt --name mybridge01 --network mybridge ubuntu`
  `docker container run -dt --name mybridge02 --network mybridge ubuntu`
## Automatic DNS resolution
Enter container using user defined bridge network and ping another container using user defined bridge network, the ping will succeed. If you're not in user defined bridge network, it'll fail.

```
docker container exec -it mybridge01 bash
/# apt-get update && apt-get install net-tools && apt-get install iputils-ping 
/# ping mybridge02
```
# Host Network
It behaves as if container is the host itself.
# None Network
- No IP address of container except the loopback interface.
- Container can't contact any other containers or external world.
- Thus this disables networking stack on container.
# Publish all argument for exposed ports
```
docker container run -dt --name webserver -p 80:80 nginx
```
## Publish all -P
All exposed ports are published to random ports of the host.
```
 docker container run -dt --name webserver nginx
 docker container run -dt -p 8080:80 --name webserver01 nginx
 docker container run -dt -P --name webserver02 nginx
```
# Legacy Approach for Linking Containers
![](_resources/Pasted%20image%2020231118155426.png)
link is telling to link container01 with container02.
![](_resources/Pasted%20image%2020231118155519.png)
You could ping "container" which is container01 from container02.
