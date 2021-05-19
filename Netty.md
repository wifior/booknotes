### 认识I/O
##### linux网络I/O模型

1. 阻塞I/O模型

2. 非阻塞I/O模型

3. I/O复用模型

4. 信号驱动I/O模型

5. 异步I/O

##### I/O复用的应用场景
- 服务器需要同时处理多个处于监听状态或者多个连接状态的套接字
- 服务器需要同时处理多种网络协议的套接字

​      支持I/O多路复用的系统调用有select,pselect,poll,epoll.
select缺陷是单个进程所打开的FD是有一定限制的,由FD_SETSIZE设置,默认1024,epoll没有这个限制.具体可通过cat /proc/sys/fs/file -max察看.

​      传统的select/poll的别一个弱点就是在socket很大时,select/poll每次都是线性扫描全部,这就导致线性下降.

​        epoll是根据每个fd上的callback函数实现的,只有活跃的socket才会调用 callback函数,其他的不会.

### NIO入门

​      与socket类和ServerSocket类相对应,NIO也提供了SocketChannel和ServerSocketChannel两种不同的套接字通道实现.它支持阻塞和非阻塞两种模式.
 ##### 缓冲区Buffer

​        Buffer是一个对象,包含一些要写入或者要读出的数据.在面向流的I/O中,可以将数据直接写入或者将数据直接读到Stream对象中.

​       在NIO库中,所有的数据都是用缓冲区来处理的,缓冲区实质上是一个数组,通常是一个字节数组.最常用的缓冲区是ByteBuffer,在java中,基本类型除Boolean外都对应有一种缓冲区.

##### 通道Channel

​      Channel是一个通道,就像自来水管,网络数据通过Channel读取和写入.通过是双向的,流是单向的.

​     Channel可以分为两大类:用于网络读写的**SelectableChannel**和用于文件操作的**FileChannel**.

##### 多路复用器Selector

​       Selector会不断的轮询注册在其上的Channel,channel上发生读写事件,就会处于就绪状态,会被Selector轮询出来,然后通过SelectionKey可以获取就绪Channel的集合,进行后续I/O操作.

​      JDK使用epoll()代替传统的select实现,所以没有最大句柄限制.

```java
ServerSocketChannel ssc = ServerSocketChannel.open();//打开ServerSocketChannel,用于监听客户端的连接
ssc.socket().bind(new InetSocketAddress(InetAddress.getByName("IP"),port));//绑定端口,
ssc.configureBlcking(false);//设置为非阻塞模式

Selector selector = Selector.open();//创建多路复用器
New Thread(new ReactorTask()).start();//启动线程

SelectionKey key = ssc.register(selector,SelectionKey.OP_ACCEPT,ioHandler);//将ssc注册到Reactor线程的多路复用器selector上





```










