## 什么叫线程安全？举例说明
当一个类被多个线程访问的时，这个类始终能表现出**正确的行为**，那么就称这个类是线程安全的。     
比如无状态对象一定是线程安全的。   
有状态对象，就是有数据存储功能的对象。有状态对象，就是有示例变量的对象，可以保存数据，是非线程安全的。  
无状态对象，就是不能保存数据。就是没有成员变量的对象，不能保存数据，是线程安全的。   

##  进程和线程的区别
进程是对运行时程序的封装，可以保存程序的运行状态，实现操作系统的并发；  
线程是进程的子任务，保证程序的实时性；  
进程是操作系统资源的分配单位，线程是CPU调度的基本单位；  
进程让操作系统的并发性成为可能，而线程让进程的内部并发成为可能。  
一个进程虽然包括多个线程，但是这些线程是共同享有进程占有的资源和地址空间的。

## volatile的理解
一旦一个共享变量（类的成员变量、类的静态成员变量）被 volatile 修饰后，那么就具备了两层语义：  
1、保证了不同线程对共享变量进行操作时的可见性，即一个线程修改了某个变量的值，这个新值对其他线程来说是立即可见的。  
2、禁止进行指令重排序。  
synchronized 关键字是防止多个线程同时执行一段代码，那么就会很影响程序执行效率；而 volatile 关键字在某些情况下性能要优于 synchronized，但是要注意 volatile 关键字是无法替代 synchronized 关键字的，因为 volatile 关键字无法保证操作的原子性。  
关键字 volatile 是线程同步的轻量级实现，主要使用的场合是: 在多线程环境下及时感知共享变量的修改，并使得其他线程可以立即得到变量的最新值。

## synchronized的理解
synchronized**内置锁**是一种**对象锁**(锁的是对象而非引用)，作用粒度是对象，可以用来实现对临界资源的同步互斥访问，是**可重入**的。特别地，对于临界资源有：  
1、若该资源是静态的，即被static关键字修饰，那么访问它的方法必须是同步且是静态的，synchronized块必须是class锁 类锁；  
2、若该资源是非静态的，即没有被 static 关键字修饰，那么访问它的方法必须是同步的，synchronized 块是实例对象锁；  
关键字synchronized 主要包含两个特征：  
互斥性：保证在同一时刻，只有一个线程可以执行某一个方法或某一个代码块。（即保证了原子性）
可见性：保证线程工作内存中的变量与公共内存中的变量同步，使多线程读取共享变量时可以获得最新值的使用。

## Lock和Synchronized的选择
总的来说，Lock 和 Synchronized 有以下几点不同：  
1、Lock是一个接口，是JDK层面的实现；而synchronized是Java中的关键字，是Java的内置特性，是JVM层面的实现；  
2、synchronized 在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；  
3、Lock 可以让等待锁的线程响应中断，而使用synchronized时，等待的线程会一直等待下去，不能够响应中断；  
4、通过Lock可以知道有没有成功获取锁，而synchronized却无法办到；  
5、Lock可以提高多个线程进行读操作的效率。  
在性能上来说，如果竞争资源不激烈，两者的性能是差不多的。而当竞争资源非常激烈时（即有大量线程同时竞争），此时Lock的性能要远远优于synchronized。Lock更加灵活。所以说，在具体使用时要根据适当情况选择。
		
## ThreadLocal
ThreadLocal 又名 **线程局部变量** ，是 Java 中一种较为特殊的**线程绑定机制**，可以为每一个使用该变量的线程都提供一个变量值的副本，并且每一个线程都可以独立地改变自己的副本，而不会与其它线程的副本发生冲突。如果一个某个变量要被某个线程 **独享**，那么我们就可以通过ThreadLocal来实现线程本地存储功能。  
get()方法是用来获取 ThreadLocal变量 在当前线程中保存的值。  
set() 用来设置 ThreadLocal变量 在当前线程中的值  
remove() 用来移除当前线程中相关 ThreadLocal变量  
initialValue() 是一个 protected 方法，一般需要重写  
1、每个线程内部有一个 ThreadLocal.ThreadLocalMap 类型的成员变量 threadLocals，这个 threadLocals 存储了与该线程相关的所有 ThreadLocal 变量及其对应的值（”ThreadLocal 变量及其对应的值” 就是该Map中的一个 Entry）。我们知道，Map 中存放的是一个个 Entry，其中每个 Entry 都包含一个 Key 和一个 Value。在这里，就threadLocals 而言，它的 Entry 的 Key 是 ThreadLocal 变量， Value 是该 ThreadLocal 变量对应的值；  
2、初始时，在Thread里面，threadLocals为空，当通过ThreadLocal变量调用get()方法或者set()方法，就会对Thread类中的threadLocals进行初始化，并且以当前ThreadLocal变量为键值，以ThreadLocal要保存的值为value，存到 threadLocals；  
3、然后在当前线程里面，如果要使用ThreadLocal对象，就可以通过get方法获得该线程的threadLocals，然后以该ThreadLocal对象为键取得其对应的 Value，也就是ThreadLocal对象中所存储的值。  
Java为了最小化减少内存泄露的可能性和影响，在ThreadLocal进行get、set操作时会清除线程Map里所有key为null的value。所以最怕的情况就是，ThreadLocal对象设null了，开始发生“内存泄露”，然后使用线程池，线程结束后被放回线程池中而不销毁，那么如果这个线程一直不被使用或者分配使用了又不再调用get/set方法，那么这个期间就会发生真正的内存泄露。因此，最好的做法是：在不使用该ThreadLocal对象时，及时调用该对象的remove方法去移除ThreadLocal.ThreadLocalMap中的对应Entry。

## 为什么线程通信的方法wait(), notify()和notifyAll()被定义在Object类里？
Wait-notify机制是在获取对象锁的前提下不同线程间的通信机制。在Java中，任意对象都可以当作锁来使用，由于锁对象的任意性，所以这些通信方法需要被定义在Object类里。

## 为什么wait(), notify()和notifyAll()必须在同步方法或者同步块中被调用？
wait/notify机制是依赖于Java中Synchronized同步机制的，其目的在于确保等待线程从Wait()返回时能够感知通知线程对共享变量所作出的修改。如果不在同步范围内使用，就会抛出java.lang.IllegalMonitorStateException的异常。wait()表示让当前线程释放对象锁并进入阻塞状态，所以要先确保在同步范围内才能获得对象锁。notify()表示唤醒一个等待相应对象锁的线程，并在当前线程释放后释放对象锁。

## Java原子性操作实现原理
使用循环CAS实现原子性操作，CAS是在操作期间先比较旧值，如果旧值没有发生改变，才交换成新值，发生了变化则不交换。这种方式会产生以下几种问题：1.ABA问题，通过加版本号解决；2.循环时间过长开销大，一般采用自旋方式实现；3. 只能保证一个共享变量的原子操作。 
什么是自旋转？  
试想下，如果两个线程资源竞争不是特别激烈，而处理器阻塞一个线程引起的线程上下文的切换的代价高于等待资源的代价的时候（锁的已保持者保持锁时间比较短），那么线程B可以不放弃CPU时间片，而是在“原地”忙等，直到锁的持有者释放了该锁，这就是自旋锁的原理，可见自旋锁是一种非阻塞锁。

## sleep() 和 wait() 区别
答：sleep()方法是线程类（Thread）的静态方法，导致此线程暂停执行指定时间，将执行机会给其他线程，但是监控状态依然保持，到时后会自动恢复（线程回到就绪（ready）状态），因为调用 sleep 不会释放对象锁。wait() 是 Object 类的方法，对此对象调用 wait()方法导致本线程放弃对象锁(线程暂停执行)，进入等待此对象的等待锁定池，只有针对此对象发出 notify 方法（或 notifyAll）后本线程才进入对象锁定池准备获得对象锁进入就绪状态。

## sleep() 和 yield() 区别
① sleep() 方法给其他线程运行机会时**不考虑线程的优先级**，因此会给低优先级的线程以运行的机会；yield() 方法**只会给相同优先级或更高优先级**的线程以运行的机会；
② 线程执行 sleep() 方法后转入**阻塞**（blocked）状态，而执行 yield() 方法后转入**就绪（ready）**状态；
③ sleep() 方法声明抛出InterruptedException，而 yield() 方法没有声明任何异常；
④ sleep() 方法比 yield() 方法（跟操作系统相关）具有更好的可移植性。

## 避免死锁的常见方法：
1)避免一个线程同时获取多个锁
2)避免一个线程在锁内同时占用多个资源，尽量保证每个锁只占用一个资源
3)尝试使用定时锁，使用lock.tryLock(timeout)来替代使用内部锁机制。
4)对数据库锁，加锁和解锁必须在一个数据库连接里，否则会出现解锁失败的情况。

## 什么是死锁？死锁的必要条件？怎么克服？
答：
死锁是指两个以上的线程永远阻塞的情况，这种情况产生至少需要两个以上的线程和两个以上的资源。  
产生死锁的四个必要条件：  
**互斥条件**：一个资源每次只能被一个进程使用。  
**请求与保持条件**：一个进程因请求资源而阻塞时，对已获得的资源保持不放。  
**不剥夺条件**:进程已获得的资源，在末使用完之前，不能强行剥夺。  
**循环等待条件**:若干进程之间形成一种头尾相接的循环等待资源关系。  
这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。  

死锁的解决方法:
a 撤消陷于死锁的全部进程；
b 逐个撤消陷于死锁的进程，直到死锁不存在；
c 从陷于死锁的进程中逐个强迫放弃所占用的资源，直至死锁消失。
d 从另外一些进程那里强行剥夺足够数量的资源分配给死锁进程，以解除死锁状态

## CountDownLatch(闭锁) 与CyclicBarrier(栅栏)的区别
CountDownLatch: 允许一个或多个线程等待其他线程完成操作. CyclicBarrier：阻塞一组线程直到某个事件发生。 
1. 闭锁用于等待事件、栅栏是等待线程.
2. 闭锁CountDownLatch做减计数，而栅栏CyclicBarrier则是加计数。
3. CountDownLatch是一次性的，CyclicBarrier可以重用。
4. CountDownLatch一个线程(或者多个)，等待另外N个线程完成某个事情之后才能执行。CyclicBarrier是N个线程相互等待，任何一个线程完成之前，所有的线程都必须等待。 
CountDownLatch 是计数器, 线程完成一个就记一个,就像报数一样, 只不过是递减的.
而CyclicBarrier更像一个水闸, 线程执行就像水流, 在水闸处都会堵住, 等到水满(线程到齐)了, 才开始泄流.

## execute 和submit的区别
Execute()用于提交不需要返回值得任务，submit()用于提交需要返回值的任务，发挥Future类型的对象。

## Shutdown和shutdownNow的区别
它们的原理都是遍历线程池中的工作线程，然后逐个调用线程的Internet方法来中断线程，所以无法响应中断的任务可能永远无法终止。
ShutdownNow首先将线程池的状态设置成STOP,然后尝试停止所有正在执行或暂停的任务，并返回等待执行任务的列表。而shutdown只是将线程池设置成SHUTDOWN状态，然后中断没有正在执行任务的线程。




## 什么是线程池（thread pool）
答：在面向对象编程中，创建和销毁对象是很费时间的，因为创建一个对象要获取内存资源或者其它更多资源。在 Java 中更是如此，虚拟机将试图跟踪每一个对象，以便能够在对象销毁后进行垃圾回收。所以提高服务程序效率的一个手段就是尽可能减少创建和销毁对象的次数，特别是一些很耗资源的对象创建和销毁，这就是"池化资源"技术产生的原因。线程池顾名思义就是事先创建若干个可执行的线程放入一个池（容器）中，需要的时候从池中获取线程不用自行创建，使用完毕不需要销毁线程而是放回池中，从而减少创建和销毁线程对象的开销。

http://www.cnblogs.com/dolphin0520/p/3932921.html
## ConcurrentHashMap实现原理
ConcurrentHashMap和Hashtable主要区别就是围绕着锁的粒度以及如何锁。
Hashtabl在竞争激烈的环境下表现效率低下的原因是一把锁锁住整张表，导致所有线程同时竞争一个锁。ConcurrentHashMap采用分段锁，每把锁锁住容器中的一个Segment。那么多线程访问容器里不同的Segment的数据时线程就不会存在竞争，从而有效提高并发访问效率。首先是将数据分层多个Segment存储，并为每个Segment分配一把锁，当一个线程范围其中一段数据时，其他线程可以访问其他段的数据。

数据结构：
ConcurrentHashMap内部是有Segment数组和HashEntry数组组成。一个ConcurrentHashMapMap里包含一个Segment数组，而Segment的结构和HashMap一样，里面是由一个数组和链表结构组成，所以一个Segment内部包含一个HashEntry数组。每个HashEntry是一个链表结构，对于HashEntry数组进行修改时首先需要获取与它对应的Segment锁。默认情况下有16个Segment

Segment的定位:
使用Wang/Jenkins hash变种算法对元素的hashCode进行一次再散列，目的是为了减少散列冲突。

ConcurrentHashMap的操作:
1.) get
get操作实现非常简单高效。先经过一次在散列，然后用这个散列值通过散列运算定位到Segment，再通过散列算法定位到元素。get之所以高效是因为整个get过程不需要加锁，除非读到空值才会加锁重读。实现该技术的技术保证是保证HashEntry是不可变的。
第一步是访问count变量，这是一个volatile变量，由于所有的修改操作在进行结构修改时都会在最后一步写count 变量，通过这种机制保证get操作能够得到几乎最新的结构更新。对于非结构更新，也就是结点值的改变，由于HashEntry的value变量是 volatile的，也能保证读取到最新的值。接下来就是根据hash和key对hash链进行遍历找到要获取的结点，如果没有找到，直接访回null。
对hash链进行遍历不需要加锁的原因在于链指针next是final的。但是头指针却不是final的，这是通过getFirst(hash)方法返回，也就是存在 table数组中的值。这使得getFirst(hash)可能返回过时的头结点，例如，当执行get方法时，刚执行完getFirst(hash)之后，另一个线程执行了删除操作并更新头结点，这就导致get方法中返回的头结点不是最新的。这是可以允许，通过对count变量的协调机制，get能读取到几乎最新的数据，虽然可能不是最新的。要得到最新的数据，只有采用完全的同步。
2.) put
该方法也是在持有段锁(锁定当前segment)的情况下执行的，这当然是为了并发的安全，修改数据是不能并发进行的，必须得有个判断是否超限的语句以确保容量不足时能够rehash。首先根据计算得到的散列值定位到segment及该segment中的散列桶中。接着判断是否存在同样一个key的结点，如果存在就直接替换这个结点的值。否则创建一个新的结点并添加到hash链的头部，这时一定要修改modCount和count的值，同样修改count的值一定要放在最后一步。
3）remove
HashEntry中除了value不是final的，其它值都是final的，这意味着不能从hash链的中间或尾部添加或删除节点，因为这需要修改next 引用值，所有的节点的修改只能从头部开始。对于put操作，可以一律添加到Hash链的头部。但是对于remove操作，可能需要从中间删除一个节点，这就需要将要删除节点的前面所有节点整个复制一遍，最后一个节点指向要删除结点的下一个结点。
首先定位到要删除的节点e。如果不存在这个节点就直接返回null，否则就要将e前面的结点复制一遍，尾结点指向e的下一个结点。e后面的结点不需要复制，它们可以重用。
4) size()
每个Segment都有一个count变量，是一个volatile变量。当调用size方法时，首先先尝试2次通过不锁住segment的方式统计各个Segment的count值得总和，如果两次值不同则将锁住整个ConcurrentHashMap然后进行计算。
参见《java并发编程的艺术》P156
http://www.cnblogs.com/ITtangtang/p/3948786.html

## 线程的几种可用状态
线程在运行周期里有6中不同的状态。
1. New 新建
2. RUNNABLE 运行状态,操作系统中运行与就绪两种状态统称运行中
3. BLOCKED 阻塞状态
4. WAITING	等待状态
5. TIME_WAITING 超时等待
6. TERMINATED	终止状态

## 同步方法和同步代码块的区别是什么
1. 同步方法只能锁定当前对象或class对象， 而同步方法块可以使用其他对象、当前对象及当前对象的class作为锁。
2. 从反编译后的结果看，对于同步块使用了monitorenter和monitorexit指令，而同步方法则是依靠方法上的修饰符ACC_SYNCHRONIZED来完成，但它们的本质都是对一个对象监视器进行获取，而这个获取过程是排他的。
	
### 显示锁ReentrantLock与内置锁synchronized的相同与区别
相同：显示锁与内置锁在加锁和内存上提供的语义相同(互斥访问临界区)
不同：
1.使用方式，内置无需指定释放锁，简化锁操作。显示锁拥有锁获取和释放的可操作性。
2.功能上：显示锁提供了其他很多功能如定时锁等待、可中断锁等待、公平性、尝试非阻塞获取锁、以及实现非结构化的加锁。（一个线程获取不到锁而被阻塞在synchronized之外时，对该线程进行中断操作，此时该线程的中断表示为会被修改，但线程依旧会被阻塞在synchronized上，等待获取锁。）
3.对死锁的处理：内置只能重启，显示可以通过设置超时获取锁来避免
4.性能上：java1.5 显示远超内置，java1.6 显示锁稍微比内置好

7. atomicinteger和Volatile等线程安全操作的关键字的理解和使用

SOF你遇到过哪些情况。

21. 实现多线程的3种方法：Thread与Runable。
**1)继承Tread类，重写run函数**
**2)实现Runnable接口**
**3)实现Callable接口**


22. 线程同步的方法：sychronized、lock、reentrantLock等。

23. 锁的等级：方法锁、对象锁、类锁。
26. ThreadPool用法与优势。

26 Callable和Runnable的区别
27. Concurrent包里的其他东西：ArrayBlockingQueue、CountDownLatch等等。

29. foreach与正常for循环效率对比。
31. 反射的作用于原理。

32. 泛型常用特点，List<String>能否转为List<Object>。

36. 设计模式：单例、工厂、适配器、责任链、观察者等等。
37. JNI的使用。
java的代理是怎么实现的  
35. Java1.7与1.8新特性。
lmbda表达式
Java8新特性
连接池使用使用什么数据结构实现
实现连接池
结束一条 Thread 有什么方法？ interrupt 底层实现有看过吗？线程的状态是怎么样的？如果给你实现会怎么样做？
Java 中有内存泄露吗？是怎么样的情景？为什么不用循环计数？
java都有哪些加锁方式
AIO与BIO的区别
生产者与消费者，手写代码
1. Java创建线程之后，直接调用start()方法和run()的区别
2. 常用的线程池模式以及不同线程池的使用场景
3. newFixedThreadPool此种线程池如果线程数达到最大值后会怎么办，底层原理。
4. 多线程之间通信的同步问题，synchronized锁的是对象，衍伸出和synchronized相关很多的具体问题，例如同一个类不同方法都有synchronized锁，一个对象是否可以同时访问。或者一个类的static构造方法加上synchronized之后的锁的影响。
5. 了解可重入锁的含义，以及ReentrantLock 和synchronized的区别
6. 同步的数据结构，例如concurrentHashMap的源码理解以及内部实现原理，为什么他是同步的且效率高
8. 线程间通信，wait和notify

9. 定时线程的使用
10. 场景：在一个主线程中，要求有大量(很多很多)子线程执行完之后，主线程才执行完成。多种方式，考虑效率。
14. 并发、同步的接口或方法
16. J.U.C下的常见类的使用。 ThreadPool的深入考察； BlockingQueue的使用。（take，poll的区别，put，offer的区别）；原子类的实现。
17. 简单介绍下多线程的情况，从建立一个线程开始。然后怎么控制同步过程，多线程常用的方法和结构
19. 实现多线程有几种方式，多线程同步怎么做，说说几个线程里常用的方法


## 写出生产者消费者模式。

    public class ProducerConsumerPattern {
        public static void main(String args[]){
         BlockingQueue sharedQueue = new LinkedBlockingQueue();
         Thread prodThread = new Thread(new Producer(sharedQueue));
         Thread consThread = new Thread(new Consumer(sharedQueue));
         prodThread.start();
         consThread.start();
        }
    }
    
    //Producer Class in java
    class Producer implements Runnable {
        private final BlockingQueue sharedQueue;
        public Producer(BlockingQueue sharedQueue) {
            this.sharedQueue = sharedQueue;
        }
        @Override
        public void run() {
            for(int i=0; i<10; i++){
                try {
                    System.out.println("Produced: " + i);
                    sharedQueue.put(i);
                } catch (InterruptedException ex) {
                    Logger.getLogger(Producer.class.getName()).log(Level.SEVERE, null, ex);
                }
            }
        }
    }
     
    //Consumer Class in Java
    class Consumer implements Runnable{
        private final BlockingQueue sharedQueue;
        public Consumer (BlockingQueue sharedQueue) {
            this.sharedQueue = sharedQueue;
        }
        @Override
        public void run() {
            while(true){
                try {
                    System.out.println("Consumed: "+ sharedQueue.take());
                } catch (InterruptedException ex) {
                    Logger.getLogger(Consumer.class.getName()).log(Level.SEVERE, null, ex);
                }
            }
        }
    }







