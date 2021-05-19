Spring 注入方式

​     属性注入

​        

```xml
<bean id = "car" class="com.test.beans.car">
    <property name="name" value="baoma"></property>
    <property name="name" ref="引用的类"></property>
</bean>    
```

​    构造器注入

```xml
<bean id = "car" class="com.test.beans.car">
    <constuctor-arg type="java.lang.String">
        <value><![CDATA[<shanghai>]]></value>
    </constructor-arg> 
</bean>
```











































