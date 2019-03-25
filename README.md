# k8s-ubuntu
Kubernetes cluster on Ubuntu

# Configure the Master Node
## install docker, kubeadm, kubelet, kubectl 
```
$ apt-get update && apt-get install -y apt-transport-https
$ curl -s https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
$ apt update && apt install -qy docker-ce
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
$ echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" \
    > /etc/apt/sources.list.d/kubernetes.list
$ apt-get update && apt-get install -y kubeadm kubelet kubectl
```

## turn off swap
`swapoff -a`

## Configure Kubernetes
* `kubeadm init --apiserver-advertise-address=<MASTER_IP> --pod-network-cidr=10.244.1.0/16`
* get message (example):
```
Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join 192.168.0.15:6443 --token puvb9w.85mnirj5i8nceyaa --discovery-token-ca-cert-hash sha256:<HASH>
```
## Create a new user
```
adduser ubuntu
usermod -aG sudo ubuntu
su - ubuntu
```

## Set up the Kubernetes config as the new user:
```
$ mkdir -p ~/.kube
$ sudo cp -i /etc/kubernetes/admin.conf ~/.kube/config
$ sudo chown $(id -u):$(id -g) ~/.kube/config
```

## Again, as the new user, deploy a flannel network to the cluster:
`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`



# Configure the Worker Nodes

## install docker, kubeadm, kubelet, kubectl 

```
$ apt-get update && apt-get install -y apt-transport-https
$ curl -s https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
$ apt update && apt install -qy docker-ce
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
$ echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" \
    > /etc/apt/sources.list.d/kubernetes.list
$ apt-get update && apt-get install -y kubeadm kubelet kubectl
```

## turn off swap
`swapoff -a`

## Configure Kubernetes
`kubeadm join 192.168.0.15:6443 --token puvb9w.85mnirj5i8nceyaa --discovery-token-ca-cert-hash sha256:<HASH>`

# Check
* `kubectl get nodes`
* `kubectl get pods --all-namespaces`

# dashboard
* `kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml`
* `kubectl proxy`

# Config Gitlab
* `kubectl get secrets`
* 




# links
* https://mherman.org/blog/setting-up-a-kubernetes-cluster-on-ubuntu/
* https://linuxconfig.org/how-to-install-kubernetes-on-ubuntu-18-04-bionic-beaver-linux
