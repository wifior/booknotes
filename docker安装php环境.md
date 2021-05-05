### docker安装php、mysql、nginx

---

#### 1.php镜像安装

```xml
docker search php
docker pull php:7.2-fpm
```

2.mysql镜像安装

```xml
docker search mysql
docker pull mysql
```

3.nginx镜像安装

```xml
docker search nginx
docker pull 
```

安装完镜像后，我们在本地创建容器运行目录

```xml
mkdir /web/www
mkdir /web/server/mysql|nginx
```

创建容器

```xml
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v /web/server/mysql/:/var/lib/mysql -d mysql
```

说明：

```tex
–name 参数为mysql容器名称，可以自己定义。 
-p 指定外部映射到容器的端口 
-e 环境变量 MYSQL_ROOT_PASSWORD为指定root账号密码 
-v 映射目录或者文件 
    * /data 为mysql数据目录 
    * /conf 为配置目录 
-d 以守护进程的方式运行
```

查看容器

```xml
docker ps -a
```

删除容器

```xml
docker rm [container id]
```



