## 面试经历

### 远图
1. nginx几种负载均衡的算法,用ip_hash有什么缺点
2. RabbitMQ在项目中的应用场景是什么？内部的原理是什么？如果不小心连续发送两条消息该怎么办？
3. Redis、webservice、websocket大致讲一下   ✅
4. hashmap hashtable concurrenthashmap 红黑树 log n   
5. http一个协议里包含哪几部分  get post的区别   ✅
6. 线程实现的方式，最好选哪种   ✅
7. 说说java的反射
8. 说说arraylist 和 linkedlist   ✅
10. 说一说索引			✅
11. mysql存储引擎   ✅
12. 数据库集群主从？ ✅ mysql复制功能，主从、 双主模式、分库设计、分表设计。
13. 秒杀问题（高并发、超卖） ✅


## 群核科技
1. TCP三次握手、四次分手，TIME_WAITE是主动发送端的状态还是被动接收端的状态。 HTTP协议是由谁主动断开的？服务器隔一段时间后会主动断开。
2. arraylist linkedlist vector
3. spring   
4. 三道算法题 
1) 二分法，找到和目标数最接近的那个数， 如果需要找到C个和目标数最接近的数，求这个范围  
2）数据流中的中位数 见剑指offer倒数第二题  
3）5X5的数组中，里面的数字都是杂乱的，可以从任意位置出发，每次只能走一步，向上，向下，向左或者向右，不能重复走，不能越界，每次只能往比自己数字小的地方走，求最长的路径

## 群核科技现场面
1. redis为什么要设计成单线程
2. rabbitmq和其他的区别，消息队列的选型。
3. 重哈希的时候为什么会出现死循环，JDK8里的hashmap有哪些改进，为什么要改。
4. 有哪些查找的方式
5. 了解下自动化容器 docker k8s

## 神汽在线
1. lvs和nginx的区别。

## 阿里
1. 索引，你为什么要这么设计索引，索引一般设计在哪些地方，比方说where后面有三个查询字段，你会怎么设置索引
2. JDK1.8有些改进，我说了lambda表达式、hashmap啥的，他说 你要讲出你在实际开发过程中（写代码过程）你能直观感受到的一些变化。。。 stream api
3. 一亿条用户数据每条有一个id，归属地字段，如何快速找出某个id对应的归属地。不用数据库。  布隆过滤器 。 storm系统中可以添加不同的功能节点，包括统计、过滤
4. 比如三台服务器做负载，某一台服务器突然响应变慢，你会怎么去排查这个问题，为什么要去看cpu占有率。
5. 如何重写一个equal方法，比如给你一个类，你去重写equal方法你会怎么做。 有哪些原则 effective java最深的一个例子
6. 如果让你设计一个大型网站你怎么设计
7. 在项目中如何做到线上不停服更新的
8. 单例模式中线程安全的几个体现
9. 乐观锁和悲观锁
10. 我项目中 nginx给三台服务器做负载，如果其中一台挂了，还是会有请求分发到挂掉的那一台上，这时候怎么办。
11. volatile的应用场景

## 海康
1. 消息队列中 消息有哪些类型 
2. 索引的原理 为什么索引适合范围查询

## 网易 邮件事业部
1. spring bean的destroy方法是什么时候执行的？ 容器关闭的时候
2. 消息队列有没有遇到过消息丢失、重复发送这样的情况
3. 服务器异常是如何调试的
4. 了解下dubbo
5. 了解下读写 数据库之间的切换

## 网易 游戏部
1. 有一张表，有名字、班级、分数。求出每门课都大于80分的所有学生名字。 对学生进行分组，分组之后用min聚合函数求出该学生分数最低课程 大于80 即可。
2. RabbitMQ有个很重要的指标？Consumer Utilisation   https://www.bbsmax.com/A/QV5ZambJyb/   
3. 消息队列做持久化吗  http://blog.csdn.net/u013256816/article/details/60875666
4. 事务的嵌套问题。做了事务的方法A调用 另一个类的做了事务的方法B，？  Spring 中7种事务传播行为，默认PROPAGATION_REQUIRED ，如果当前没有事务，就新建一个事务，如果已经存在一个事务，就加入到这个事务中。  http://stamen.iteye.com/blog/1441794
5. 在一个切点中，多个切面的执行顺序  http://xdwangiflytek.iteye.com/blog/1936114
6. 服务中分布式事务问题。一个事务先调了服务A，再调了服务B。
7. webservice wsdl   http://www.cnblogs.com/JeffreySun/archive/2009/12/14/1623766.html
8. 数据库的事务。

## 奇点云
1. hashtable 的get加了sychronized了吗， 加了
2. 线程a输出1，线程b输出2，线程c输出3，a,b,c同时开启，如何保证依次输出a,b,c  http://www.cnblogs.com/icejoywoo/archive/2012/10/15/2724674.html
3. OAuth2.0  https://www.jianshu.com/p/d74ce6ca0c33
4. http的优缺点，