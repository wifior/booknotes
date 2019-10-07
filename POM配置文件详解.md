# POM配置文件详解

> 国庆期间，不想出去凑热闹，就宅在家里了。原本想写个关于spring boot的文章，但突然想到之前面试时的一个问题，虽然maven一直在用，pom文件常用的一些属性也都知道，但还是有一些即使用了，也不知道什么意思的。所以就打算总结一下。虽然网上很多pom文件详解的文章，但我还是想以由简入深方式来聊一下。

### 一、Maven仓库的设置

​    Maven的仓库分为远程仓库和本地仓库;

- **远程仓库**

​       远程仓库又称为中央仓库，在你下载好maven不做任何配置时，就是使用的中央仓库的配置，不过有时候网络会很慢，所以一般我设置国内阿里云的镜像站。

```xml
<mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
    <mirror> 
      <id>alimaven</id>  
      <name>aliyun maven</name>  
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
      <mirrorOf>central</mirrorOf> 
    </mirror> 
  </mirrors>
```

- **本地仓库**

  本地仓库就是你在远程仓库上下载到本地的jar包存储路径。未配置maven时,路径为${user.home}/.m2/repository,下面是我的配置

  ```xml
    <!-- localRepository
     | The path to the local repository maven will use to store artifacts.
     |
     | Default: ${user.home}/.m2/repository-->
    <localRepository>D:\Maven</localRepository>
  ```

  ### 二、pom文件常用配置



