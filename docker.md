# Docker简介

- Docker**是什么**

​        程序即运行，仓库，镜像，容器。

- 镜像/容器

​        可以把容器看做是一个简易版的Linux系统

- 安装

    ```shell
    centos 6.8安装
    yum install -y epel-release
    yum install -y docker-io 
    配置文件：/etc/sysconfig/docker
    启动 ：service docker start
    ```
    
- 阿里云镜像

```xml
/etc/sysconfig/docker（centos 6.8）
/etc/docker/daemon.json (centos 7)
```

- 常用命令

    ```xml
docker --version
docker info
docker --help
docker images -a  列出本地所有镜像(含中间镜像层)
docker images -q  显示镜像ID
docker images --digests   显示镜像的摘要信息
docker images --no-trunc 显示完整 的镜像信息
docker search <image name> 搜索镜像仓库里的应用
docker pull <image name>:<version> 拉取镜像
docker rmi [-f] <image name>:<version>  删除应用 -f强制  
docker run [-d|-t|-i|-P|-p] <image name>  运行镜像，-d后台运行容器，-i以交互模式运行与-t同时使用，-t为容器分配一个伪输入终端,-P随机端口映射，-p指定端口映射
    ```

- docker run 和docker start区别

```tex
docker run 后面指定的是一个镜像
而docker start指定的是一个容器
```

https://vuepress.mirror.docker-practice.com/image/pull.html





