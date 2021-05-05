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



















