# myBatic源码

## myBatis编程步骤

1.创建SqlSessionFactory对象.

2.通过SqlSessionFactory获取SqlSession对象.

3.通过SqlSession获得Mapper代理对象.

4.通过Mapper代理对象,执行数据库操作.

5.执行成功,则使用SqlSession提交事务.

6.执行失败,则使用SqlSession回滚事务.

7.最终,关闭会话.

## #{}和${}的区别是什么?

${}是Properties文件中的变量占位符,可用于XML标签属性值和SQL内部,属于**字符串替换**.

#{}是SQL的参数占位符,Mybatis会将SQL中的`#{}`替换为`?`号,在SQL执行前会使用PreparedStatement的参数设置方法,按序给SQL的`?`号占位符设置参数值.可有效防止SQL注入.

## 当实体类中的属性名和表中的字段名不一样,怎么办?

第一种,使用别名；

第二种,Mybatis配置,下划线转驼峰风格

```xml
<setting name="logImpl" value="LOG4J"/>
    <setting name = "mapUnderscoreToCamelCase" value="true" />
</settings>
```

第三种,通过<resultMap>来映射字段名和实体类属性名的一一对应的关系.

## xml映射文件中,都有哪些标签?

**常见的标签:**select|insert|update|delete

<cache />标签,给定命名空间的缓存配置. <cache-ref />其他命名空间缓存配置的引用.

<resultMap/>标签,是用来描述如何从数据库结果集中来加载对象.

<sql />标签,可被其他语句引用的可重用语句块,<include /> 标签,引用<sql/>标签的语句.

<selectKey/>标签,不支持自增的主键生成策略标签.

**动态的sql:**

- <if/>

- <choose />  | <when /> | <otherwise />

- <trim /> | <where /> | <set />  
- <foreach />
- <bind />

## xml映射文件与Mapper接口对应关系的原理是什么？

Mapper接口，对应的关系如下：

- 接口的全限名，就是映射文件中的"namespace"的值
- 接口的方法名，就是映射文件中MappedStatement的"id"值
- 接口方法内的参数，就是传递给SQL的参数

Mapper接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位一个对应的MappedStatement.

Mapper接口的实现类，通过MyBatis使用JDK Proxy自动生成其代理对象Proxy,而代理对象Proxy会拦截接口方法，从而“调用”对应的MappedStatement方法，最终执行SQL,返回执行结果。

## Mapper 接口绑定有几种实现方式？

第一种：通过**XML Mapper**里面写SQL来绑定，这种情况下，要指定XML映射文件里面的"namespace"必须为接口的全路径名。（推荐）

第二种: 通过**注解**绑定,就是 在接口的方法上面加上`@Select`、`@Update`、`@Insert`、`@Delete`注解，里面包含sql语句来绑定。

第三种：是第二种的特例，也是通过注解，在接口的方法上加上@SelectProvider、@UpdateProvider、@InsertProvider、@DeleteProvider注解，通过Java代码，生成对应的动态SQL。

## Mapper中传递多个参数

```java
<!--第一种:-->
// 调用方法
Map<String, Object> map = new HashMap();
map.put("start", start);
map.put("end", end);
return studentMapper.selectStudents(map);

// Mapper 接口
List<Student> selectStudents(Map<String, Object> map);

// Mapper XML 代码
<select id="selectStudents" parameterType="Map" resultType="Student">
    SELECT * 
    FROM students 
    LIMIT #{start}, #{end}
</select>
=========================================================
<!---第二种，使用@Param注解 ，推荐此方式->
  // 调用方法
return studentMapper.selectStudents(0, 10);

// Mapper 接口
List<Student> selectStudents(@Param("start") Integer start, @Param("end") Integer end);

// Mapper XML 代码
<select id="selectStudents" resultType="Student">
    SELECT * 
    FROM students 
    LIMIT #{start}, #{end}
</select>

<!---第三种，直接传递->
 // 调用方法
return studentMapper.selectStudents(0, 10);

// Mapper 接口
List<Student> selectStudents(Integer start, Integer end);

// Mapper XML 代码
<select id="selectStudents" resultType="Student">
    SELECT * 
    FROM students 
    LIMIT #{param1}, #{param2}
</select>   
```

## MyBatis有哪些Executor执行器？

Mybatis有四种Executor执行器，分别是SimpleExecutor、ReuseExecutor、BatchExecutor、CachingExecutor。

- SimpleExecutor:每执行一次update或select操作，就创建一个Statement对象，用完立刻关闭Statement对象。
- ReuseExecutor:执行update或select操作，以SQL作为key查找缓存的Statement对象，存在就使用，不存在就创建；用完后，不关闭Statement对象，而是放置于缓存`Map<String,Statement>`内，供下一次使用。简言之，就是重复使用Statement对象。
- BatchExecutor:执行update操作(无select，因为jdbc批处理不支持select操作)，将所有sql都添加到批处理中（通过addBatch方法），等待统一执行（使用executeBatch方法），它缓存了多个Statement对象，每个Statement对象都是调用addBatch方法完毕后，等待一次执行executeBatch批处理。实际上，整个过程与JDBC批处理是相同。
- CachingExecutor:在上述的三个执行器之上，增加二级缓存的功能。

通过设置<setting name="defaultExecutorType" value="">的`"value"`属性，可传入`SIMPLE`、`REUSE`、`BATCH`三个值，分别使用SimpleExecutor、ReuseExecutor、BatchExecutor执行器。

通过设置<setting name="cacheEnabled" value="">的`"value"`属性为`true`时，创建CachingExecutor执行器。

## Mybatis的延迟加载

Mybatis仅支持association关联对象和collection关联集合对象的延迟加载。其中，association指的就是一对一，collection指的就是一对多查询。

在Mybatis配置文件中，可以配置<setting name="lazyLoadingEnabled" value="true" />来启用延迟加载的功能。默认情况延迟加载是关闭的。

**原理：**使用CGLIB或Javassist（默认）创建目标对象的代理对象。当调用代理对象的延迟加载属性的getting方法时，进入拦截器方法。比如调用`a.getB().getName()`方法,进入拦截器的`invoke(...)`方法，发现`a.getB()`需要延迟加载时，那么就会单独发送事先保存好的查询关联B对象的SQL，把B查询上来，然后调用`a.setB(b)`方法，于是a对象b属性就有值了，接着完成a.getB().getName()方法的调用。这就是延迟加载的基本原理。





