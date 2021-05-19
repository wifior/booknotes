### Docker安装Java环境

---

#### 1.docker下安装centos

```shell
docker pull centos   
docker run -i -t -d centos /bin/bash
docker ps -a
docker attach [CONTAINER ID]
yum install net-tools.x86_64
```

2.安装wget 

```tex
yum install wget -y
```

3.安装jdk

<https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>

下载时要oracle帐号密码,网上百度找一个就好了.

```xml
vi /etc/profile
===========================
export JAVA_HOME=/opt/jdk
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
============================
source /etc/profile
```



- ### 错误汇总

```shell
# Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
systemctl daemon-reload
systemctl restart docker.service


```

```shell
FROM centos
MAINTAINER The Centos and Jdk project <soft119@qq.com>

```

- ### Dockerfile

```shell
FROM centos:7

MAINTAINER ghjun

rum mkdir /opt/jdk

workdir /opt/jdk
# 把宿主机jar拷贝到容器中并解压
add jdk-8u241-linux-x64.tar.gz /opt/jdk
# 配置环境变量
env JAVA_HOME /opt/jdk/jdk1.8.0_241
env JRE_HOME /opt/jdk/jdk1.8.0_241/jre
env PATH $JAVA_HOME/bin:$PATH
# 宿主机当前目录下执行行命令创建镜像  
docker build -t jdk1.8 .
```

















