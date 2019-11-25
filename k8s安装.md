## 设置系统主机名及host文件的相互解析

```xml
hostnamectl set-hostname k8s-master01
```

## 安装依赖包

```xml
yum install -y conntrack ntpdate ntp ipvsadm ipset jq iptables curl sysstat libseccomp wget vim net-tools git
```

## 设置防火墙为iptables 并设置空规则

```xml
systemctl stop firewalld && systemctl disable firewalld
yum -y install iptables-services && systemctl start iptables && systemctl enable iptables && iptables -F && service iptables save
```

## 关闭selinux

```xml
swapoff -a && sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab setenforce 0 && sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

调整内核参数，对于 K8S

```xml
cat > kubernetes.cnf <<EOF
net.bridge.bridge-nf-call-iptables=1
net.bridge.bridge-nf-call-ip6tables=1
net.ipv4.ip_forward=1
net.ipv4.tcp_tw_recycle=0
vm.swappiness=0 # 禁止使用swap空间，只 有当系统 OOM时才允许使用
vm.overcommit_memory=1 # 不检查物理内存是否够用
vm.panic_on_oom=0 # 开启OOM
fs.inotify.max_user_instances=8192
fs.inotify.max_user_watches=1048576
fs.file-max=52706963
fs.nr_open=52706963
net.ipv6.conf.all.disable_ipv6=1
net.netfilter.nf_conntrack_max=2310720
EOF
cp kubernetes.conf /etc/sysctl.d/kubernetes.conf
sysctl -p /etc/sysctl.d/kubernetes.conf
```

## 调整系统时区

```she
# 设置系统时区为中国上海
timedatectl set-timezone Asia/Shanghai
#将当前的UTC写入硬件时钟
timedatectl set-local-rtc 0
#重启依赖于系统时间的服务
systemctl restart rsyslog
systemctl restart crond
```

## 关闭系统不需要服务

```she
systemctl stop postfix && systemctl disable postfix
```

## 升级系统内核到4.44

```shel
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.e17.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install -y kernel-lt
grub2-set-default "Centos Linux (4.4.182-1.e17.elrepo.x86_64) 7 (Core)"
```



