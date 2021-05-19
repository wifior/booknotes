JVM学习

## 1.了解jvm内存模型

​    在学习jvm之前，我们先对jvm有个整体的了解，下图为java虚拟机运行时数据区，这些区域都有各自的用途，以及创建和销毁时间。

![](C:\Users\Think\Desktop\jvm1.jpg)

-  **程序计数器**

​           此区域是一块较小的内存空间，主要用途就是当前线程所执行的字节码的行号指示器。本区域为线程私有的内存，在执行native方法时，这个计数器的值为Undefined,这个区域也是唯一一个在java虚拟机规范中没有规定任何OutOfMemoryError情况的区域。

- **虚拟机栈**

​        它的生命周期与线程相同；它描述的是java方法执行的内存模型，每个方法执行时都会创建一个栈帧用于存储方法从入栈到出栈所用到的局部变量表、操作数栈，动态链接，方法出口等。

​        局部变量表存放的是编译期可知的各种基本数据类型，对象引用和returnAddress类型（一条字节码指令的地址）。其中64位长度的long和double类型数据占用2个局部变量空间，其他占用一个。

- **本地方法栈**

   此区域是虚拟机使用到Native方法服务。

- **堆**

  此区域为java虚拟机所管理的内存中最大的一块。也是的有线程共享的一块区域，在虚拟机启动时创建。本区域唯一目的就是存放对象实例。

  ​    **JDK1.7的堆内存模型**

  ​         jdk1.7的堆内存模型分为年轻代（该区域分为三部分，Eden区和两个大小严格相同的Survivor区），老年代和永久代。
  
  ​    **JDK1.8的堆内存模型**
  
  ​        jdk1.8分为年轻代、老年代和Metaspace（元数据空间）区域。Metaspace区域所占用的内存空间不是在虚拟机内部，而是在本地内存空间中。
  
  此区域也是垃圾收集器管理的主要区域，现在的收集器基本都采用分代收集算法，所以可细分为新生代和老年代。再细分，可以分为eden区，from空间，to空间等。
  
  ![](C:\Users\Think\Desktop\jvm3.jpg)

## 垃圾回收算法

在上述的虚拟机内丰的其他几个运行区域内都有发生OutOfMemoryError异常的可能。我们可以通过参数-XX:+HeapDumpOnOutOfMemoryError让虚拟机在出现内存溢出异常时Dump出当前的内存堆转储快照，以便后续分析。分析我们将在后面学习，本节我们学习垃圾回收算法。

- **标记-清除算法**

​        本算法是最基础的算法，分为两个阶段，标记和清除阶段，缺点是效率不高和空间不连续问题。

- **复制算法**

​        该算法将内存分为两个大小相等的两块，运行高效，但利用率不高

- **标记-整理算法**

​        与标记-清除算法一样，只是后续步骤不是直接 对可回收的对象进行清理，而是让所有存活的对象都向一端移动，然后清理。

- **分代收集算法**

​        将java堆分主新生代和老年代，这样可以根据各个年代的特点采用适当的收集算法。

## 垃圾收集器

有了垃圾回收算法，垃圾收集器就是算法的具体实现。

## jvm优化

- **参数类型**

​            *标准参数* 

​                  -help

​                  -version        

​           *-X参数(非标准参数)*

​                 -Xint

​                 -Xcomp

​           *-XX参数(使用率高)*

​                 -XX:newSize

​                -XX:+UseSerialGC

- **实例说明** 

​        ***通过-D设置系统属性参数***

​            java -Dstr=hello Test

​        ***-server与-client参数***

​            server vm的初始堆空间会大一些，默认采用并行垃圾回收器，启动运行快

​            client vm的初始堆空间会小一些，使用串行垃圾回收器，运行速度比server模式慢一些

​            32位系统： windows系统，不论硬件如何，默认采用client类型的vm,其他操作系统，机器内丰2GB以上默认采用server模式，否则是client模式。

​            64位系统：只有server类型，不支持client类型

​        ***-Xint 、-Xcomp 、-Xmixed***

​            在解释 模式下，-Xint 标记会强制JVM执行所有的字节码，-Xcomp参数与-Xint正好相反，jvm在第一次使用时会把所有的字节码编译成本地。-Xmixed是混合模式，将解释模式和编译模式进行混合使用，由jvm自己决定，推荐使用。

​           ***-XX参数***【主要用于JVM调做优】

​             -XX参数分为boolean类型和非boolean类型

​             boolean类型：-XX:[+-]<name>

​            非boolean类型：-XX:<namae>=<value>

​        ***-Xms与-Xmx参数***

​          -Xms：堆内存初始大小，等价于-XX:MaxHeapSize

​          -Xmx：堆内存最大大小，等价于-XX:InitalHeapSize

​       ***-XX:PrintFlagsFinal*** 

​           -XX:PringFlagsFinal  打印运行时参数，=为默认值，:=为值修改过的

​       ***jinfo命令***【查看正在运行的jvm参数】

```java
jps -l //-l显示全包
jinfo -flags <进程ID>
```
​      ***jstat命令***【查看堆内存使用情况】

```java
jstat [-命令选项] [vmid] [间隔时间/毫秒] [查询次数]
jstat -class <进程ID>  //查看class加载统计
jstat -compiler <进程ID>   //查看编译统计
jstat -gc <进程ID>  //垃圾回收统计
```
​      ***jmap命令***【查看内存使用情况及内存溢出分析】

```java
jmap -heap <进程ID>    //查看内存使用情况
jmap -histo <进程ID>|more    //查看所有对象，包括活跃以及非活跃---对象说明
jmap -histo:live <进程ID> | more //只查看活跃对象
jmap -dump:format=b,file=dumpfilename <进程ID>   //将内存使用情况dump到文件中
    
对象说明：
 B byte
 C char
 D double
 F float
 I int
 J long
 Z boolean
 [  数组，如[I 表示int[]
 [L+类名 其他对象
 
```

​       ***jhat命令***【对dump文件进行分析】

```java
jhat -port <port> <file>  //端口为一会要用浏览器访问，此处可使用OQL语句
```

​     ***MAT工具对dump文件进行分析***

```xml
//http://www.eclipse.org/mat下载地址
MAT:MemoryAnalyzer

```



![1568900903769](C:\Users\Think\AppData\Roaming\Typora\typora-user-images\1568900903769.png)

​     ***jstack的作用***

        ```xml
jstack <进程ID>
        ```

​    ***visualVM远程监控***

​        监控tomcat要修改bin下的catalina.sh里的参数

        ```xml
-Dcom.sun.management.jmxremote  :允许使用jmx远程管理
-Dcom.sun.management.jmxremote.port=9999  :jmx远程连接端口
-Dcom.sun.management.jmxremote.authenticate=false  :不进行身份验证，任何用户都可以连接
-Dcom.sun.management.jmxremote.ssl=false  :不使用ssl
        ```



































