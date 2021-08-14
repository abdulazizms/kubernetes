# kubernetes setup

# Configure sysctl

\# vim /etc/sysctl.d/kubernetes.conf

- add lines below to the file

net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

\# vim /etc/modules-load.d/kubernetes.conf

- add line below to the file

br_netfilter

\# sysctl --system

# Install kubelet, kubeadm and kubectl on all nodes

1. Add the Google repository key.

\# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

2. Add the Google repository.

\# vim /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io kubernetes-xenial main

3. Update the list of packages and install kubelet, kubeadm and kubectl.

\# apt-get update

- install latest version of kubelet, kubeadm and  kubectl

\# apt-get install kubelet kubeadm kubectl

- or install specific version of kubelet, kubeadm and  kubectl

\# apt install -y kubeadm=1.21.4-00 kubelet=1.21.4-00 kubectl=1.21.4-00

- to get version list

\# apt list -a kubeadm

4. Disable the swap.

\# swapoff -a
\# sed -i '/ swap / s/^/#/' /etc/fstab

# Install container runtime on all nodes (docker/containerd/cri-o)

- for example using containerd

1. Install containerd

\# apt-get install -y containerd.io

2. Configure containerd

\# mkdir -p /etc/containerd

3. Start and enable service containerd

\# systemctl start containerd

\# systemctl enable containerd

# Init kubernetes master node

## - for single master node

\# kubeadm init --pod-network-cidr 192.168.0.0/16

## - for multi master node, init from one of master

\# kubeadm init --control-plane-endpoint "loadbalancer-ip-masters:6443" --upload-certs --pod-network-cidr 192.168.0.0/16

notes: for multi master, load balancer is needed for load balance the master nodes.

# Join another master (multi master) or worker

- join another master to cluster

\# kubeadm join loadbalancer-ip-masters:6443 --token [token] --discovery-token-ca-cert-hash [token] --control-plane --certificate-key [key]

- join worker to cluster

get output from this command from master node

\# kubeadm token create --print-join-command

- join worker for single master

\# kubeadm join ip-master:6443 --token [token] --discovery-token-ca-cert-hash [token]

- join worker for multi master

\# kubeadm join loadbalancer-ip-master:6443 --token [token] --discovery-token-ca-cert-hash [token]
