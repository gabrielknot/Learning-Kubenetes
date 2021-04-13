# Learning-Kubenetes
A simple repository to help me to learn kubernetes.

## 0 - Before install kubeadm, kubectl and kubelet You shold unactive your firewall if are using your personal machine to training.
Ofcourse just in case your cluster is working, and just if you dont know yet how manage your firewall ports currectly.

## 0.5 - Almost forgot:
You have to inactive your pc swap partionament.

## 1 - After install kubeadm and kbectl, you may have to unactive your firewall, following.


```console
foo@user:~$ sudo systemctl stop firewalld
```
## 2 - Then configure the docker correctly

```console
foo@user:~$ cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

```

## 3 - Now you may can use kubeadm currectly!!

First of all you may have to init your kubeadm, but you dont have a podnetwork configured yet. In this case the pod`s can`t comunicate with each other. 
So you have to set up the your server ipadress in the flag --apiserver-advertise-address=<ipadress> and the --pod-network-cidr, in this case we will use flannel.

```console
foo@user:~$ sudo kubeadm init --apiserver-advertise-address=<ipadress> --pod-network-cidr=10.244.0.0/16
```

### You can get your ipadress in the first result of:

```console
foo@user:~$ hostname -I
```
## 4 - Get and execute the 3 following comands:

```console
foo@user:~$ mkdir -p $HOME/.kube
foo@user:~$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
foo@user:~$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
## 5 - After you may have to apply the pod-networ, following:
```console
foo@user:~$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
