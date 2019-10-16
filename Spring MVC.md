# Spring MVC

### 一、认识web.xml

**1.作用**

web.xml在web项目中并非必须，它主要作用是用来配置欢迎页、servlet、filter、listener等以及定制servlet、JSR、Context初始化参数。

**2.加载顺序**

context-param->listener->filter->servlet

### 二、Spring MVC中web.xml配置

- **编写"(servlet-name)"-servlet.xml**:servlet-name必须是标签<servlet-name>指定的值。
- **添加servlet定义配置DispatcherServlet:**前端控制器，spring的核心要素。







