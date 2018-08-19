##zookeeper+黑板模式
黑板模式允许多个消息读写者同时存在，消息的生产者和消费者完全分开。这就像一个黑板，任何一个教授（消息的生产者）都可以在其上书写消息，任何一个学生（消息的消费者）都可以从黑板上读取消息，两者在空间和时间上可以解耦，并且互不干扰。  
zookeeper是一个分布式应用程序协调服务、开源的组件，有分布式服务的基本都可以用zookeeper。简单来说就是令分布式应用程序在系统中同时运行。  
提供了很多服务包括命名服务 配置管理 集群管理 节点领导者选举 锁定和同步服务 数据注册表。   
zookeeper有三种角色的节点，分别是Leader（领导者）、Follower（跟随者）、Observer（观察者）。  
Leader 负责更新系统状态，进行投票（选举leader）的发起和决议。   
Follower 用于接收客户端请求并向客户端返回处理请求的结果，在选举过程中参与投票。
Observer 可以接收客户端的连接，将写请求转发给Leader的那，但Observer不参加投票过程，只同步Leader的状态。Observer的目的是为了扩展系统，提高读取速度。   
为什么要进行Leader选举？ Leader主要作用是保证分布式数据一致性，即每个节点的存储的数据同步。

##Spring Security
关于安全框架最重要的就是两点 区别 Authentication（鉴权） 和 Authorization（授权）  
简而言之，鉴权就是鉴定用户“是谁”，而授权则是鉴定用户“是否可以”做这件事情或访问某个URL。当然授权的前提是明确知道当前的用户“是谁”，所以一般情况下鉴权发生在授权之前。   
在Spring Security中，匿名用户会被鉴定为anonymousUser，并且拥有ROLE_ANONYMOUS角色，所以，当未登录用户访问一个在Spring Security管辖范围内，但被配置成permitAll的URL时，如果你通过SecurityContextHolder.getContext().getAuthentication()方法获取当前用户，你将会得到一个匿名用户的对象。这个时候访问Authentication.getName()将会得到anonymousUser，而Authentication.getAuthorities()将会返回一个只有ROLE_ANONYMOUS单个元素的列表。这是匿名用户的场景。   
Spring Security是通过一系列javax.servlet.Filter来实现的，所以在配置Spring Security时，如果将URL mapping到Spring Security的Filter的filter-mapping之外的的话，就属于Spring Security的管辖范围之外了。Spring Security对这些URL鞭长莫及。

##Hadoop
基础编程模型：把一个大任务拆分成小任务，再进行汇总  
MR任务：Job = Map + Reduce   
    Map的输出是Reduce的输入、MR的输入和输出都是在HDFS  
    
##HBase
什么是BigTable？: 把所有的数据保存到一张表中，采用冗余 ---> 好处：提高效率   
1、因为有了bigtable的思想：NoSQL：HBase数据库  
2、HBase基于Hadoop的HDFS的  
3、描述HBase的表结构  
核心思想是：利用空间换效率  

##Spark
Spark 运算比 Hadoop 的 MapReduce 框架快的原因是因为 Hadoop 在一次 MapReduce 运算之后,会将数据的运算结果从内存写入到磁盘中,第二次 Mapredue 运算时在从磁盘中读取数据,所以其瓶颈在2次运算间的多余 IO 消耗. Spark 则是将数据一直缓存在内存中,直到计算得到最后的结果,再将结果写入到磁盘,所以多次运算的情况下, Spark 是比较快的

##Storm
storm与hadoop的比较：  
1.Storm用于实时计算，Hadoop用于离线计算。  
2. Storm处理的数据保存在内存中，源源不断；Hadoop处理的数据保存在文件系统中，一批一批。  
3.  Storm的数据通过网络传输进来；Hadoop的数据保存在磁盘中。  
4.  Storm与Hadoop的编程模型相似。