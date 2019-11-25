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

# 具体使用

- ### log4j

 **1.maven依赖**

```xml
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>
```

**2.配置文件(log4j.properties)**

```xml
log4j.rootLogger = debug, console
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} %m%n
```

3.使用

```java
public class Log4jTest {
	private static final Logger logger=Logger.getLogger(Log4jTest.class);
	public static void main(String[] args){
		if(logger.isTraceEnabled()){
			logger.debug("log4j trace message");
		}
		if(logger.isDebugEnabled()){
			logger.debug("log4j debug message");
		}
		if(logger.isInfoEnabled()){
			logger.debug("log4j info message");
		}
	}
}
```



- ### commons-logging与log4j1集成

**1.maven依赖**

```xml
<dependency>
	<groupId>commons-logging</groupId>
	<artifactId>commons-logging</artifactId>
	<version>1.2</version>
</dependency>
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>
```

**2.配置文件(log4j.properties)**

```xml
log4j.rootLogger = trace, console
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} %m%n
```

**3.具体使用**

```java
private static Log logger=LogFactory.getLog(Log4jJclTest.class);

public static void main(String[] args){
	if(logger.isTraceEnabled()){
		logger.trace("commons-logging-log4j trace message");
	}
	if(logger.isDebugEnabled()){
		logger.debug("commons-logging-log4j debug message");
	}
	if(logger.isInfoEnabled()){
		logger.info("commons-logging-log4j info message");
	}
}
```



- ### slf4j与log4j1集成

1.maven依赖

```xml
<!-- slf4j -->
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-api</artifactId>
	<version>1.7.12</version>
</dependency>

<!-- slf4j-log4j -->
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-log4j12</artifactId>
	<version>1.7.12</version>
</dependency>

<!-- log4j -->
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>
```

**2.配置文件(log4j.properties)**

```xml
log4j.rootLogger = debug, console
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} %m%n
```

**3.具体使用**

```java
private static Logger logger=LoggerFactory.getLogger(Log4jSlf4JTest.class);

public static void main(String[] args){
	if(logger.isDebugEnabled()){
		logger.debug("slf4j-log4j debug message");
	}
	if(logger.isInfoEnabled()){
		logger.info("slf4j-log4j info message");
	}
	if(logger.isTraceEnabled()){
		logger.trace("slf4j-log4j trace message");
	}
}
```



------



- ### log4j2

**log4j2分成2个部分**

- log4j-api： 作为日志接口层，用于统一底层日志系统
- log4j-core : 作为上述日志接口的实现，是一个实际的日志框架

**1.maven依赖**

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.2</version>
</dependency>
<dependency>
	<groupId>org.apache.logging.log4j</groupId>
	<artifactId>log4j-core</artifactId>
	<version>2.2</version>
</dependency>
```

2.配置文件(log4j2.xml/json/yml)

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
  </Appenders>
  <Loggers>
    <Root level="debug">
      <AppenderRef ref="Console"/>
    </Root>
  </Loggers>
</Configuration>
```

**3.使用**

```java
private static final Logger logger=LogManager.getLogger(Log4j2Test.class);

public static void main(String[] args){
	if(logger.isTraceEnabled()){
		logger.debug("log4j trace message");
	}
	if(logger.isDebugEnabled()){
		logger.debug("log4j debug message");
	}
	if(logger.isInfoEnabled()){
		logger.debug("log4j info message");
	}
}
```



- ### commons-logging与log4j2集成

**1.maven依赖**

```xml
<dependency>
	<groupId>commons-logging</groupId>
	<artifactId>commons-logging</artifactId>
	<version>1.2</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.2</version>
</dependency>
<dependency>
	<groupId>org.apache.logging.log4j</groupId>
	<artifactId>log4j-core</artifactId>
	<version>2.2</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-jcl</artifactId>
    <version>2.2</version>
</dependency>
```

**2.配置文件(log4j2.xml)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
  </Appenders>
  <Loggers>
    <Root level="debug">
      <AppenderRef ref="Console"/>
    </Root>
  </Loggers>
</Configuration>
```

**3.具体使用**

```java
private static Log logger=LogFactory.getLog(Log4j2JclTest.class);

public static void main(String[] args){
	if(logger.isTraceEnabled()){
		logger.trace("commons-logging-log4j trace message");
	}
	if(logger.isDebugEnabled()){
		logger.debug("commons-logging-log4j debug message");
	}
	if(logger.isInfoEnabled()){
		logger.info("commons-logging-log4j info message");
	}
}
```

------



- ### slf4j与log4j2集成

1.maven依赖

```xml
<!-- slf4j -->
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-api</artifactId>
	<version>1.7.12</version>
</dependency>
<!-- log4j2 -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.2</version>
</dependency>
<dependency>
	<groupId>org.apache.logging.log4j</groupId>
	<artifactId>log4j-core</artifactId>
	<version>2.2</version>
</dependency>
<!-- log4j-slf4j-impl -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>2.2</version>
</dependency>
```

**2.配置文件(log4j2.xml)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
  </Appenders>
  <Loggers>
    <Root level="debug">
      <AppenderRef ref="Console"/>
    </Root>
  </Loggers>
</Configuration>
```

**3.具体使用**

```java
private static Logger logger=LoggerFactory.getLogger(Log4j2Slf4jTest.class);

public static void main(String[] args){
	if(logger.isTraceEnabled()){
		logger.trace("slf4j-log4j2 trace message");
	}
	if(logger.isDebugEnabled()){
		logger.debug("slf4j-log4j2 debug message");
	}
	if(logger.isInfoEnabled()){
		logger.info("slf4j-log4j2 info message");
	}
}
```



------



- ### logback

**1.maven依赖**

```xml
<dependency> 
	<groupId>ch.qos.logback</groupId> 
	<artifactId>logback-core</artifactId> 
	<version>1.1.3</version> 
</dependency> 
<dependency> 
    <groupId>ch.qos.logback</groupId> 
    <artifactId>logback-classic</artifactId> 
    <version>1.1.3</version> 
</dependency>
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-api</artifactId>
	<version>1.7.12</version>
</dependency>
```

**2.配置文件(logback.xml)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <root level="DEBUG">          
    <appender-ref ref="STDOUT" />
  </root>  
  
</configuration>
```

**3.使用**

```java
private static final Logger logger=LoggerFactory.getLogger(LogbackTest.class);

public static void main(String[] args){
	if(logger.isDebugEnabled()){
		logger.debug("slf4j-logback debug message");
	}
	if(logger.isInfoEnabled()){
		logger.debug("slf4j-logback info message");
	}
	if(logger.isTraceEnabled()){
		logger.debug("slf4j-logback trace message");
	}
	
	LoggerContext lc = (LoggerContext) LoggerFactory.getILoggerFactory();
    StatusPrinter.print(lc);
}
```

### 

- ### commons-logging与logback集成

**1.maven依赖**

```xml
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>jcl-over-slf4j</artifactId>
	<version>1.7.12</version>
</dependency>
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-api</artifactId>
	<version>1.7.12</version>
</dependency>
<dependency> 
	<groupId>ch.qos.logback</groupId> 
	<artifactId>logback-core</artifactId> 
	<version>1.1.3</version> 
</dependency> 
<dependency> 
    <groupId>ch.qos.logback</groupId> 
    <artifactId>logback-classic</artifactId> 
    <version>1.1.3</version> 
</dependency>
```

**2.配置文件(logback.xml)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>
  <root level="DEBUG">          
    <appender-ref ref="STDOUT" />
  </root>  
</configuration>
```

**3.具体使用**

```java
private static Log logger=LogFactory.getLog(LogbackTest.class);

public static void main(String[] args){
	if(logger.isTraceEnabled()){
		logger.trace("commons-logging-jcl trace message");
	}
	if(logger.isDebugEnabled()){
		logger.debug("commons-logging-jcl debug message");
	}
	if(logger.isInfoEnabled()){
		logger.info("commons-logging-jcl info message");
	}
}
```

- ### slf4j与logback集成

1.maven依赖

```xml
<!-- slf4j-api -->
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-api</artifactId>
	<version>1.7.12</version>
</dependency>
<!-- logback -->
<dependency> 
	<groupId>ch.qos.logback</groupId> 
	<artifactId>logback-core</artifactId> 
	<version>1.1.3</version> 
</dependency> 
<dependency> 
    <groupId>ch.qos.logback</groupId> 
    <artifactId>logback-classic</artifactId> 
    <version>1.1.3</version> 
</dependency>
```

2.配置文件(logback.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>
  <root level="DEBUG">          
    <appender-ref ref="STDOUT" />
  </root>  
</configuration>
```

3.具体使用

```java
private static final Logger logger=LoggerFactory.getLogger(LogbackTest.class);

public static void main(String[] args){
	if(logger.isDebugEnabled()){
		logger.debug("slf4j-logback debug message");
	}
	if(logger.isInfoEnabled()){
		logger.info("slf4j-logback info message");
	}
	if(logger.isTraceEnabled()){
		logger.trace("slf4j-logback trace message");
	}
}

```



------



- ### slf4j与jdk-logging集成

**1.maven依赖**

```xml
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-api</artifactId>
	<version>1.7.12</version>
</dependency>
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-jdk14</artifactId>
	<version>1.7.12</version>
</dependency>
```

**2.具体使用**

```java
private static final Logger logger=LoggerFactory.getLogger(JulSlf4jTest.class);

public static void main(String[] args){
	if(logger.isDebugEnabled()){
		logger.debug("jul debug message");
	}
	if(logger.isInfoEnabled()){
		logger.info("jul info message");
	}
	if(logger.isWarnEnabled()){
		logger.warn("jul warn message");
	}
}
```



