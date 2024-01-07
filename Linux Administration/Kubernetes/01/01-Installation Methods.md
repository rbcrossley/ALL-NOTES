There are multiple ways to get started with fully functional kubernetes environment.
1) Use the managed kubernetes service
2) Use minikube
3) Install and configure kubernetes manually (hard way)

# Managed Kubernetes Service
Various cloud providers provide managed Kubernetes clusters. Most of the organizations prefer to make use of this approach.

# Minikube
- Single-node kubernetes cluster
# Install and configure Kubernetes Manually(Hard Way)

# Installation Aspects
## kubectl
CLI for running user commands against cluster.
## Kubernetes Master
kubernetes cluster by itself.
## Worker Node Agents
Kubernetes Node Agent.
![](_resources/Pasted%20image%2020231212150841.png)
k8s master=control plane.
worker node agents is where application is running.

# kubectl
- used to deploy applications
- inspect and manage cluster resources
- view logs
![](_resources/Pasted%20image%2020231212152139.png)

### kubeconfig
To connect to the Kubernetes Master, there are two important data which kubectl needs:
- DNS/IP of the cluster
- Authentication Credentials
![](_resources/Pasted%20image%2020231212152231.png)
## Install kubectl using native package management

1) Add the Kubernetes `yum` repository. If you want to use Kubernetes version different than v1.28, replace v1.28 with the desired minor version in the command below.

```bash
# This overwrites any existing configuration in /etc/yum.repos.d/kubernetes.repo
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.28/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.28/rpm/repodata/repomd.xml.key
EOF
```
2) Install kubectl using yum:

```
sudo yum install -y kubectl
```
Source: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
![](_resources/Pasted%20image%2020231212170946.png)
To get proper output, we need to put kube-config.
I was creating a kubernetes cluster.
https://www.tecmint.com/install-kubernetes-cluster-on-centos-7/
Installed kubectl,kubeadm and kubelet.
Add /etc/hosts entry for communicating between the servers.
To fix unknown description error,
https://serverfault.com/questions/1124015/kubeadm-is-showing-error-cri
To join worker nodes  to master, use this command



```
To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.13.163.161:6443 --token hmaj9s.86vxo9lu559lxrqj \
        --discovery-token-ca-cert-hash sha256:b66f05d963bd3ff9412e0afd43398c61cd832d5575c7eeb8ee8f9fd4cd5fc32d

```
# Pods
To run our first container.
docker command
```
docker run --name mywebserver nginx
```
kubectl command
```
kubectl run mywebserver --image=nginx
```

# Exec into containers
docker command
```
docker exec -it mywebserver bash
```
kubectl command
```
kubectl exec -it mywebserver -- bash
```
mywebserver is the name of the pod