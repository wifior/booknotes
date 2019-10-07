# Log4j

- log4j = log for java
- log4j.properties介绍

目的地：appender

布局：layout

控制单元：logger

级别：level

```java
private static final Logger logger = Logger.getLogger(this.getClass());//log4j
private static final Logger logger = LoggerFactory.getLogger(this.getClass());//sfl4j
```

debug<info<warn<error

# 日志的发展

### 日志发展历史

​    1、1996年欧洲安全电子市场项目组推出log4j;
​    2、2002年sun推出自己的jul;
​    3、接着apache推日志接口jcl;
​    4、2006年，Ceki Gülcü离开apache，推出slf4j和logback;
​    5、2012年，apache重写log4j,推出log4j2;
​    在java日志发展的历史中，Ceki Gülcü起着非常重要的作用，他开发了log4j、slf4j和logback。

### 日志接口

- jcl

​    jcl是apache推出的。

- slf4j

​    slf4j是Ceki Gvlcv推出的。

### 日志实现

####     按接口组合

- ​    jcl和**log4j**,**log4j2**是apache的

- ​    slf4j和**logback**是Ceki Gülcü的

![](/home/ghjun/桌面/日志系统.png)

- slf4j为纯日志接口
- logback/slf4j-jdk14/slf4j-log4j12/log4j2均可认为是slf4j的实现类
- jul/log4j作为最早开始的日志系统，本身就是一种日志的实现，没有任何框架
- jul-to-slf4j/log4j-to-slf4j/jcl-over-slf4j/作为桥接接口，可以将原有接口的内容进行接收，然后通过slf4j进行输出

## 其余常见问题

- slf4j的几个实现jar包一定会冲突，尤其要注意
- slf4j-api和实现版本最好对应，尤其是1.6.x和1.5.x不兼容，直接升级到最新版本
- log4j与logback可以同时使用，logback实现slf4j的接口，与log4j没有任何关系，完全可以同时使用
- 建议选用slf4j + logback，或slf4j + log4j2
- 日志系统在正常情况下是不会影响应用性能的，但应该注意量，太大量的日志会拖累性能



