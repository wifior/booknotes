jdk dockerfile

```shell
FROM centos:latest
MAINTAINER ghjun
ADD jdk-8u251-linux-x64.tar.gz /user/local/java
ENV JAVA_HOME /user/local/java/jdk1.8.0_251
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:%JAVA_HOME/bin
CMD JAVA -version
```

docker build -t jdk1.8.0_251 .