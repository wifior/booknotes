1.查看/etc/hostname,本例采用k8s-master,k8s-node1,k8s-node2

2.使用阿云的k8s源:

```tex
$ cat /etc/apt/sources.list
# 系统安装源
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
# kubeadm及kubernetes组件安装源
deb https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial main

```

3.更新源并安装kubeadm,kubectl,kubelet软件包

```tex
apt-get update -y && apt-get install -y kubelet kubeadm kubectl --allow-unauthenticated
```

> 报错:
>
> W: GPG 错误：https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial InRelease: 由于没有公钥，无法验证下列签名： NO_PUBKEY **6A030B21BA07F4FB**
>
> 执行下面语句:
>
> sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys **6A030B21BA07F4FB**
>
> 注意:
>
> 6A030B21BA07F4FB,换成自己的,按上面提示来

4.关闭swap

```tex
sudo vi /etc/fstab
注释掉UUID和/swapfile两行

或者
sudo swapoff -a
```

5.使用kubeadmin初始化master节点

```tex
sudo docker pull mirrorgooglecontainers/kube-apiserver-amd64:v1.13.2
sudo docker pull mirrorgooglecontainers/kube-controller-manager-amd64:v1.13.2
sudo docker pull mirrorgooglecontainers/kube-scheduler-amd64:v1.13.2
sudo docker pull mirrorgooglecontainers/kube-proxy-amd64:v1.13.2
sudo docker pull mirrorgooglecontainers/pause:3.1
sudo docker pull mirrorgooglecontainers/etcd-amd64:3.2.24
sudo docker pull coredns/coredns:1.2.6

sudo docker tag docker.io/mirrorgooglecontainers/kube-proxy-amd64:v1.13.2 k8s.gcr.io/kube-proxy:v1.13.2
sudo docker tag docker.io/mirrorgooglecontainers/kube-scheduler-amd64:v1.13.2 k8s.gcr.io/kube-scheduler:v1.13.2
sudo docker tag docker.io/mirrorgooglecontainers/kube-apiserver-amd64:v1.13.2 k8s.gcr.io/kube-apiserver:v1.13.2
sudo docker tag docker.io/mirrorgooglecontainers/kube-controller-manager-amd64:v1.13.2 k8s.gcr.io/kube-controller-manager:v1.13.2
sudo docker tag docker.io/mirrorgooglecontainers/etcd-amd64:3.2.24  k8s.gcr.io/etcd:3.2.24
sudo docker tag docker.io/mirrorgooglecontainers/pause:3.1  k8s.gcr.io/pause:3.1
sudo docker tag docker.io/coredns/coredns:1.2.6  k8s.gcr.io/coredns:1.2.6
```

