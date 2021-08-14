# kubernetes setup

1- Add the Google repository key.

\# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

2- Add the Google repository.

\# vim /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io kubernetes-xenial main

3- Update the list of packages and install kubelet, kubeadm and kubectl.

\# apt-get update
\# apt-get install kubelet kubeadm kubectl

4- Disable the swap.

\# swapoff -a
\# sed -i '/ swap / s/^/#/' /etc/fstab
