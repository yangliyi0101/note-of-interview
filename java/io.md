# Java IO
## Java IO 分类
 - **Java BIO**： **同步阻塞**，服务器实现模式为**一个连接一个线程**，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。   
 - **Java NIO** ：**同步非阻塞**，服务器实现模式为**一个请求一个线程**，即当一个连接创建后，不需要对应一个线程，这个连接会被注册到**多路复用器**上面，所以所有的连接只需要一个线程就可以搞定，当这个线程中的多路复用器进行轮询的时候，发现连接上有请求的话，才开启一个线程进行处理，也就是一个请求一个线程模式。BIO与NIO一个比较重要的不同，是我们使用BIO的时候往往会引入多线程，每个连接一个单独的线程；而NIO则是使用单线程或者只使用少量的多线程，每个连接共用一个线程。   
 - **Java AIO(NIO.2)** ： 异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理。（同步IO和异步IO的关键区别反映在数据拷贝阶段是由用户线程完成还是内核完成。所以说异步IO必须要有操作系统的底层支持。）（事件驱动IO就绪通知的方式-epoll，内核发现进程指定的一个或多个IO条件就绪了，就通知进程。）
 
 BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。    
 NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。    
 AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。  
 
## 名词解释
 - **同步**:指的是用户进程触发IO操作需要等待或者轮询的去查看IO操作执行完成才能执行其他操作.这种方式性能比较差，只有一些对数据安全性要求比较高的场景中才会使用．  
 - **异步**:异步是指用户进程触发IO操作以后便开始做自己的事情，而当IO操作已经完成的时候会得到IO完成的通知（异步的特点就是通知）  
 - **阻塞**：所谓阻塞方式的意思是指, 当试图对该文件描述符进行读写时, 如果当时没有东西可读,或者暂时不可写, 程序就进入等待 状态, 直到有东西可读或者可写为止  
 - **非阻塞**：非阻塞状态下, 如果没有东西可读, 或者不可写, 读写函数马上返回, 而不会等待  

## select和epoll的区别
最主要的区别：  
**传统的select/poll每次调用都会线性扫描全部的集合，导致效率呈现线性下降。**    
poll的执行分三部分：   
（1）.将用户传入的pollfd数组拷贝到内核空间，因为拷贝操作和数组长度相关，时间上这是一个O（n）操作  
（2）.查询每个文件描述符对应设备的状态，如果该设备尚未就绪，则在该设备的等待队列中加入一项并继续查询下一设备的状态。 查询完所有设备后如果没有一个设备就绪，这时则需要挂起当前进程等待，直到设备就绪或者超时。设备就绪后进程被通知继续运行，这时再次遍历所有设备，以查找就绪设备。这一步因为两次遍历所有设备，时间复杂度也是O（n），这里面不包括等待时间......    
（3）. 将获得的数据传送到用户空间并执行释放内存和剥离等待队列等善后工作，向用户空间拷贝数据与剥离等待队列等操作的的时间复杂度同样是O（n）。   
Linux 2.6内核完全支持epoll。**epoll的IO效率不随FD数目增加而线性下降。**  
要使用epoll只需要这三个系统调用：epoll_create(2)， epoll_ctl(2)， epoll_wait(2)
epoll用到的所有函数都是在头文件sys/epoll.h中声明的，**内核实现中epoll是根据每个fd上面的callback函数实现的**。只有"活跃"的socket才会主动的去调用 callback函数，其他idle状态socket则不会。
如果所有的socket基本上都是活跃的---比如一个高速LAN环境，过多使用epoll,效率相比还有稍微的下降。但是一旦使用idle connections模拟WAN环境,epoll的效率就远在select/poll之上了。


## 参考资料
 1. http://bbym010.iteye.com/blog/2100868
 2. http://developer.51cto.com/art/201112/307463.htm
 3. http://ifeve.com/java-nio-vs-io/



