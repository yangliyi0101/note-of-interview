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

## 实现多线程的三种方式
1）继承 Thread 类，重写run()方法。  
优点：编程简单，如果需要访问当前线程，无需使用Thread.currentThread()方法，直接使用this，即可获得当前线程。
2）实现 Runnable 接口，重写了run（）方法。启动 Runnable 实例时，需要放在 Thread 中，然后调用 start() 方法。   

实际上 Thread 类本身就实现了 Runnable 接口  class Thread implements Runnable {  
优点：线程只是实现了Runnable()接口，还可以继承其他类。在这种方式下，可以多个线程共享同一个目标对象。 

3）实现 Callable 接口。Java 5 开始提供，可以返回结果（通过 Future），也可以抛出异常，需要实现的是 call() 方法，这也是和Runnable不同的地方。

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
答：sleep()方法是线程类（Thread）的静态方法，导致此线程暂停执行指定时间，将执行机会给其他线程，但是监控状态依然保持，到时后会自动恢复（线程回到就绪（ready）状态），因为调用 sleep 不会释放对象锁。  
wait() 是 Object 类的方法，对此对象调用 wait()方法导致本线程放弃对象锁(线程暂停执行)，进入等待此对象的等待锁定池，只有针对此对象发出 notify 方法（或 notifyAll）后本线程才进入对象锁定池准备获得对象锁进入就绪状态。

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

## 阻塞队列
自从Java 1.5之后，在java.util.concurrent包下提供了若干个阻塞队列，主要有以下几个：  
**ArrayBlockingQueue**：基于数组实现的一个阻塞队列，在创建ArrayBlockingQueue对象时必须制定容量大小。并且可以指定公平性与非公平性，默认情况下为非公平的，即不保证等待时间最长的队列最优先能够访问队列。  
**LinkedBlockingQueue**：基于链表实现的一个阻塞队列，在创建LinkedBlockingQueue对象时如果不指定容量大小，则默认大小为Integer.MAX_VALUE。  
**PriorityBlockingQueue**：以上2种队列都是先进先出队列，而PriorityBlockingQueue却不是，它会按照元素的优先级对元素进行排序，按照优先级顺序出队，每次出队的元素都是优先级最高的元素。注意，此阻塞队列为无界阻塞队列，即容量没有上限（通过源码就可以知道，它没有容器满的信号标志），前面2种都是有界队列。  
**SynchronousQueue**：同步移交，不是一个真正的队列，而是一种在线程之间进行的移交机制。对于非常大的或者无界的线程池，可以通过SynchronousQueue来避免排队，以及直接将任务从生产者移交给工作者线程。  
内部最关键的两个方法，**put（）和take（）**。本质上使用了Condition的 await（）和signal（）。

## 线程池（thread pool）
答：在面向对象编程中，创建和销毁对象是很费时间的，因为创建一个对象要获取内存资源或者其它更多资源。在 Java 中更是如此，虚拟机将试图跟踪每一个对象，以便能够在对象销毁后进行垃圾回收。所以提高服务程序效率的一个手段就是尽可能减少创建和销毁对象的次数，特别是一些很耗资源的对象创建和销毁，这就是"池化资源"技术产生的原因。线程池顾名思义就是事先创建若干个可执行的线程放入一个池（容器）中，需要的时候从池中获取线程不用自行创建，使用完毕不需要销毁线程而是放回池中，从而减少创建和销毁线程对象的开销。另外一个额外的好处是，当请求达到时，工作线程通常已经存在，因此不会由于等待创建线程而延迟任务的执行，从而提高了响应时间。  
java.uitl.concurrent.**ThreadPoolExecutor**类是线程池中最核心的一个类，因此如果要透彻地了解Java中的线程池，必须先了解这个类。ThreadPoolExecutor继承了AbstractExecutorService类，并提供了四个构造器，事实上，通过观察每个构造器的源码具体实现，发现前面三个构造器都是调用的第四个构造器进行的初始化工作。分析下构造函数的各个参数：  
1、**corePoolSize**：**核心池的大小**，这个参数跟后面讲述的线程池的实现原理有非常大的关系。在创建了线程池后，默认情况下，线程池中并没有任何线程，而是等待有任务到来才创建线程去执行任务，除非调用了prestartAllCoreThreads()或者prestartCoreThread()方法，从这2个方法的名字就可以看出，是预创建线程的意思，即在没有任务到来之前就创建corePoolSize个线程或者一个线程。默认情况下，**在创建了线程池后，线程池中的线程数为0**，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中；  
2、**maximumPoolSize**：**线程池最大线程数**，这个参数也是一个非常重要的参数，它表示在线程池中最多能创建多少个线程；  
3、**keepAliveTime**：表示线程没有任务执行时最多保持多久时间会终止。默认情况下，只有当线程池中的线程数大于corePoolSize时，keepAliveTime才会起作用，直到线程池中的线程数不大于corePoolSize，即当线程池中的线程数大于corePoolSize时，如果一个线程空闲的时间达到keepAliveTime，则会终止，直到线程池中的线程数不超过corePoolSize。但是如果调用了allowCoreThreadTimeOut(boolean)方法，在线程池中的线程数不大于corePoolSize时，keepAliveTime参数也会起作用，直到线程池中的线程数为0；  
4、**unit**：参数keepAliveTime的时间单位，有7种取值，在TimeUnit类中有7种静态属性：  
5、**workQueue**：一个阻塞队列，用来存储等待执行的任务，这个参数的选择也很重要，会对线程池的运行过程产生重大影响，一般来说，这里的阻塞队列有以下几种选择：ArrayBlockingQueue、LinkedBlockingQueue、SynchronousQueue。  
6、**threadFactory**：线程工厂，主要用来创建线程；  
7、handler：表示当拒绝处理任务时的策略。  
在前面我们多次提到了**任务缓存队列**，即**workQueue**，它用来存放等待执行的任务。  
workQueue的类型为BlockingQueue<Runnable>，通常可以取下面三种类型：  
1）ArrayBlockingQueue：基于数组的先进先出队列，此队列创建时必须指定大小；  
2）LinkedBlockingQueue：基于链表的先进先出队列，如果创建时没有指定此队列大小，则默认为Integer.MAX_VALUE；   
3）SynchronousQueue：这个队列比较特殊，它不会保存提交的任务，而是将直接新建一个线程来执行新来的任务。  
**饱和策略：**  
当线程池的任务缓存队列已满并且线程池中的线程数目达到maximumPoolSize，如果还有任务到来就会采取任务拒绝策略，通常有以下四种策略：  
ThreadPoolExecutor.AbortPolicy:**中止策略**。丢弃任务并抛出RejectedExecutionException异常。默认的饱和策略
ThreadPoolExecutor.DiscardPolicy：**抛弃策略**。悄悄抛弃该任务，但是不抛出异常。
ThreadPoolExecutor.DiscardOldestPolicy：**抛弃最旧策略**：抛弃下一个将被执行的任务，然后尝试重新提交新的任务。丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）  
ThreadPoolExecutor.CallerRunsPolicy：**调用者运行策略**：由调用线程处理该任务。将某些任务回退到调用者，将任务在调用execute时在主线程中执行。  
**注意点**：
如果当前线程池中的线程数目小于corePoolSize，则每来一个任务，就会创建一个线程去执行这个任务；  
如果当前线程池中的线程数目>=corePoolSize，则每来一个任务，会尝试将其添加到任务缓存队列当中，若添加成功，则该任务会等待空闲线程将其取出去执行；若添加失败（一般来说是任务缓存队列已满），则会尝试创建新的线程去执行这个任务；   
如果当前线程池中的线程数目达到maximumPoolSize，则会采取任务拒绝策略进行处理；  
如果线程池中的线程数量大于 corePoolSize时，如果某线程空闲时间超过keepAliveTime，线程将被终止，直至线程池中的线程数目不大于corePoolSize；如果允许为核心池中的线程设置存活时间，那么核心池中的线程空闲时间超过keepAliveTime，线程也会被终止。  
**设置线程池的大小**：  
如果是CPU密集型任务，就需要尽量压榨CPU，参考值可以设为 NCPU+1；  
如果是IO密集型任务，参考值可以设置为2*NCPU

## execute 和submit的区别
Execute()用于提交不需要返回值得任务，submit()用于提交需要返回值的任务，返回Future类型的对象。

## Shutdown和shutdownNow的区别
它们的原理都是遍历线程池中的工作线程，然后逐个调用线程的Internet方法来中断线程，所以无法响应中断的任务可能永远无法终止。
ShutdownNow首先将线程池的状态设置成STOP,然后尝试停止所有正在执行或暂停的任务，并返回等待执行任务的列表。而shutdown只是将线程池设置成SHUTDOWN状态，然后中断没有正在执行任务的线程。

## CountDownLatch(闭锁) 与CyclicBarrier(栅栏)的区别
在java 1.5中，提供了一些非常有用的辅助类来帮助我们进行并发编程，比如CountDownLatch，CyclicBarrier和Semaphore  
**CountDownLatch**：比如有一个任务A，它要等待其他4个任务执行完毕之后才能执行，此时就可以利用CountDownLatch来实现这种功能了。public void await() throws InterruptedException { };   //调用await()方法的线程会被挂起，它会等待直到count值为0才继续执行。   
**CyclicBarrier**：字面意思回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行。叫做回环是因为当所有等待线程都被释放以后，CyclicBarrier可以被重用。我们暂且把这个状态就叫做barrier，当调用await()方法之后，线程就处于barrier了。   
**CountDownLatch**: 允许一个或多个线程等待其他线程完成操作. **CyclicBarrier**：阻塞一组线程直到某个事件发生。     
1. 闭锁用于等待事件、栅栏是等待线程.  
2. 闭锁CountDownLatch做减计数，而栅栏CyclicBarrier则是加计数。  
3. CountDownLatch是一次性的，CyclicBarrier可以重用。  
4. CountDownLatch一个线程(或者多个)，等待另外N个线程完成某个事情之后才能执行。CyclicBarrier是N个线程相互等待，任何一个线程完成之前，所有的线程都必须等待。   
CountDownLatch 是计数器, 线程完成一个就记一个,就像报数一样, 只不过是递减的.  
而CyclicBarrier更像一个水闸, 线程执行就像水流, 在水闸处都会堵住, 等到水满(线程到齐)了, 才开始泄流.   
 
**Semaphore**翻译成字面意思为 信号量，Semaphore可以控同时访问的线程个数，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可。

## Timer和TimerTask
其实就Timer来讲就是一个调度器,而TimerTask呢只是一个实现了run方法的一个类,而具体的TimerTask需要由你自己来实现,例如这样:  

	Timer timer = new Timer();  
	timer.schedule(new TimerTask() {  
	        public void run() {  
	            System.out.println("abc");  
	        }  
	}, 200000 , 1000);
这里直接实现一个TimerTask(当然，你可以实现多个TimerTask，多个TimerTask可以被一个Timer会被分配到多个Timer中被调度，后面会说到Timer的实现机制就是说内部的调度机制)，然后编写run方法，20s后开始执行，每秒执行一次，当然你通过一个timer对象来操作多个timerTask





## ConcurrentHashMap实现原理
ConcurrentHashMap和Hashtable主要区别就是围绕着锁的粒度以及如何锁。  
Hashtabl在竞争激烈的环境下表现效率低下的原因是一把锁锁住整张表，导致所有线程同时竞争一个锁。ConcurrentHashMap采用分段锁，每把锁锁住容器中的一个Segment。那么多线程访问容器里不同的Segment的数据时线程就不会存在竞争，从而有效提高并发访问效率。首先是将数据分层多个Segment存储，并为每个Segment分配一把锁，当一个线程范围其中一段数据时，其他线程可以访问其他段的数据。  
在理想状态下，ConcurrentHashMap 可以支持 16 个线程执行并发写操作（如果并发级别设为16），及任意数量线程的读操作。  
数据结构：  
ConcurrentHashMap本质上是一个Segment数组，而一个Segment实例又包含若干个桶，每个桶中都包含一条由若干个 HashEntry 对象链接起来的链表。Segment 类继承于 ReentrantLock 类，从而使得 Segment 对象能充当锁的角色。ConcurrentHashMap不同于HashMap，它既不允许key值为null，也不允许value值为null。  
Segment的定位：根据key的hash值的高n位就可以确定元素到底在哪一个Segment中。（并发级别为16的话 n就是4）。  
总的来说，ConcurrentHashMap读操作不需要加锁的奥秘在于以下三点：  
1、用HashEntery对象的不变性来降低读操作对加锁的需求。  
2、用Volatile变量协调读写线程间的内存可见性；  
3、若读时发生指令重排序现象，则加锁重读；  

ConcurrentHashMap的**弱一致性**主要是为了提升效率，也是一致性与效率之间的一种**权衡**。要成为强一致性，就得到处使用锁，甚至是全局锁，这就与Hashtable和同步的HashMap一样了。ConcurrentHashMap的弱一致性主要体现在以下几方面：  
get操作是弱一致的：get操作只能保证一定能看到已完成的put操作；  
clear操作是弱一致的：在清除完一个segments之后，正在清理下一个segments的时候，已经清理的segments可能又被加入了数据，因此clear返回的时候，ConcurrentHashMap中是可能存在数据的。  
ConcurrentHashMap中的迭代操作是弱一致的(未遍历的内容发生变化可能会反映出来)：在遍历过程中，如果已经遍历的数组上的内容变化了，迭代器不会抛出ConcurrentModificationException异常。如果未遍历的数组上的内容发生了变化，则有可能反映到迭代过程中

http://blog.csdn.net/justloveyou_/article/details/72783008

## CopyOnWrite
CopyOnWrite容器即写时复制的容器。通俗的理解是当我们往一个容器添加元素的时候，不直接往当前容器添加，而是先将当前容器进行Copy，复制出一个新的容器，然后新的容器里添加元素，添加完元素之后，再将原容器的引用指向新的容器。这样做的好处是我们可以对CopyOnWrite容器进行并发的读，而不需要加锁，因为当前容器不会添加任何元素。所以CopyOnWrite容器也是一种读写分离的思想，读和写不同的容器。  
从JDK1.5开始Java并发包里提供了两个使用CopyOnWrite机制实现的并发容器,它们是CopyOnWriteArrayList和CopyOnWriteArraySet。  
CopyOnWrite并发容器用于读多写少的并发场景。主要有两个缺点：内存占用问题。数据一致性问题。CopyOnWrite容器只能保证数据的最终一致性，不能保证数据的实时一致性。所以如果你希望写入的的数据，马上能读到，请不要使用CopyOnWrite容器。  

http://www.cnblogs.com/dolphin0520/p/3938914.html
  
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







