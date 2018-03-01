## 集合框架
Collection：List，Set   
Map：Hashtable，HashMap，TreeMap   

Collection  是单列集合  
List   元素是有序的(元素存取是有序)、可重复  
有序的 collection，可以对列表中每个元素的插入位置进行精确地控制。可以根据元素的整数索引（在列表中的位置）访问元素，并搜索列表中的元素。可存放重复元素，元素存取是有序的。  
List接口中常用类：  
Vector：线程安全，但速度慢，已被ArrayList替代。底层数据结构是**数组结构**  
ArrayList：线程不安全，查询速度快。底层数据结构是数组结构  
LinkedList：线程不安全。增删速度快。底层数据结构是链表结构  

Set(集) 元素不可重复。  
取出元素的方法只有迭代器。**不可以存放重复元素，元素存取是无序的**。  
Set接口中常用的类  
HashSet：线程不安全，内部是基于散列函数实现,存取速度快。它是如何保证元素唯一性的呢？依赖的是元素的hashCode方法和euqals方法。   
LinkedHashSet:具有HashSet的查询速度，且内部采用双向链表**维护元素的顺序**（默认保持插入顺序排序， 如果初始化时将accessOrder设置为true将最近最少使用顺序，即最近使用的元素插在链表尾部）。元素必须实现hashCode(）方法。内部是由链表实现。  
TreeSet：线程不安全，可以对Set集合中的元素进行排序,底层采用了红黑树。元素是以二叉树的形式存放的。意味着TreeSet中的元素要实现Comparable接口。或者有一个自定义的比较器。

Map　是一个双列集合   
|--Hashtable:线程安全。底层是**哈希表数据**结构。是同步的。不允许null作为键，null作为值。  
   |--Properties:用于配置文件的定义和操作，使用频率非常高，同时键和值都是字符串。是集合中可以和IO技术相结合的对象。(到了IO在学习它的特有和io相关的功能。)   
|--HashMap:线程不安全。底层也是哈希表数据结构。是不同步的。允许null作为键，null作为值。替代了Hashtable.  
   |--LinkedHashMap: 可以保证HashMap集合有序。存入的顺序和取出的顺序一致。  
|--TreeMap：可以用来对Map集合中的键进行排序.底层是采用红黑树．  
Collection 和 Collections的区别  
Collection是集合类的上级接口，子接口主要有Set 和List、Map。   
Collections是针对集合类的一个帮助类，提供了操作集合的工具方法：一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。   

## 接口：Collection
Collection是最基本的集合接口，**一个Collection代表一组Object，即Collection的元素（Elements）**。一些Collection允许相同的元素而另一些不行。一些能排序而另一些不行。  Java SDK不提供直接继承自Collection的类，Java SDK提供的类都是继承自Collection的“子接口”如List和Set。  
所有实现Collection接口的类都必须提供**两个标准的构造函数**：无参数的构造函数用于创建一个空的Collection，有一个Collection参数的构造函数用于创建一个新的Collection，这个新的Collection与传入的Collection有相同的元素。后一个构造函数允许用户复制一个Collection。  
主要的一个接口方法：boolean add(Ojbect c)虽然返回的是boolean，但不是表示添加成功与否，这个返回值表示的意义是add()执行后，集合的内容是否改变了（就是元素的数量、位置等有无变化）。类似的addAll，remove，removeAll，remainAll也是一样的。  
用Iterator模式实现遍历集合,Collection**有一个重要的方法：iterator()，返回一个Iterator（迭代器），用于遍历集合的所有元素**。Iterator模式可以把访问逻辑从不同的集合类中抽象出来，从而避免向客户端暴露集合的内部结构。典型的用法如下：

```
		Iterator it = collection.iterator(); // 获得一个迭代器
		while(it.hasNext()) {
			Object obj = it.next(); // 得到下一个元素
		}
```

不需要维护遍历集合的“指针”，所有的内部状态都由Iterator来维护，而这个Iterator由集合类通过工厂方法生成。每一种集合类返回的Iterator具体类型可能不同，但它们都实现了Iterator接口，因此，我们不需要关心到底是哪种Iterator，它只需要获得这个Iterator接口即可，这就是接口的好处，面向对象的威力。  
要确保遍历过程顺利完成，必须保证遍历过程中不更改集合的内容（Iterator的remove()方法除外），所以，确保遍历可靠的原则是：只在一个线程中使用这个集合，或者在多线程中对遍历代码进行同步。由Collection接口派生的两个接口是List和Set。    

## Map、Set、List、Queue、Stack的特点与用法
Set集合类似于一个罐子，"丢进"Set集合里的多个对象之间没有明显的顺序。   
List集合代表元素有序、可重复的集合，集合中每个元素都有其对应的顺序索引。   
Stack是Vector提供的一个子类，用于模拟"栈"这种数据结构(LIFO后进先出)  
Queue用于模拟"队列"这种数据结构(先进先出 FIFO)。   
Map用于保存具有**"映射关系"**的数据，因此Map集合里保存着两组值   

## ArrayList 和 LinkedList 
ArrayList 和 LinkedList 都实现了 List 接口，他们有以下的不同点：  
ArrayList 是基于索引的数据接口，它的底层是数组。它可以以O(1)时间复杂度对元素进行**随机访问**。与此对应，LinkedList 是以元素列表的形式存储它的数据，每一个元素都和它的前一个和后一个元素链接在一起，在这种情况下，查找某个元素的时间复杂度是O(n)。  
相对于 ArrayList，LinkedList的**插入，添加，删除操作速度更快**，因为当元素被添加到集合任意位置的时候，不需要像数组那样重新计算大小或者是更新索引。  
LinkedList 比 ArrayList **更占内存**，因为 LinkedList 为每一个节点存储了两个引用，一个指向前一个元素，一个指向下一个元素。  

## 遍历一个List有哪些不同的方式

		List<String> strList = new ArrayList<>();
		//使用for-each循环
		for(String obj : strList){
		  System.out.println(obj);
		}
		//using iterator
		Iterator<String> it = strList.iterator();
		while(it.hasNext()){
			String obj = it.next();
			System.out.println(obj);
		}

使用**迭代器更加线程安全，因为它可以确保，在当前遍历的集合元素被更改的时候，它会抛出ConcurrentModificationException**。

## LinkedList 和PriorityQueue 的区别
**Queue中只有两个实现类LinkedList 和PriorityQueue**。他们都是属于队列，即拥有先进先出（FIFO）的特点。而这两个类的区别在于他们的排序行为并非性能。  
LinkedList 支持双向列表操作，既可以作为队列 也可以作为堆栈。  
PriorityQueue 按优先权控制队列元素的出队次序。（列表中的排序顺序是通过实现Comparable来控制的）  

## HashMap实现原理
**数据结构**： HashMap基于哈希算法，采用链地址法来避免hash冲突，所以其内部采用链表加数组数据结构，HashMap默认的初始容量是16，**负载因子是0.75**。我们通过put()和get()方法储存和获取对象。HashMap里面实现一个静态内部类Entry, Entry其重要的属性有key , value, next，hash。用该Entry定义了一个链表的数组用来存放数据，这种方法叫做链地址法，是为了避免hash冲突。  
哈希算法：  
hash() 方法用于对Key的hashCode进行重新计算，为了防止质量低下的hashCode()函数实现。由于hashMap的支撑数组长度总是 2 的幂次，通过右移可以使低位的数据尽量的不同，从而使hash值的分布尽量均匀。  
indexFor() 方法用于生成这个Entry对象的插入位置，当length为2的n次方时，h&(length - 1)就相当于对length取模，而且速度比直接取模要快得多，这是HashMap在速度上的一个优化。  
HashMap的存取实现:  
1) put  
首先会根据对象的hashCode计算得到**散列桶坐标**（数组下表），因为数组中存放的是**Entry链表结构**，所以还需要对该链表进行遍历，通过调用key.equals方法判断是否存在该key，如果存在则修改内容。如果不存在则**采用头插入**的方式在链表头部插入一个新的节点存储这个新的映射关系。  
**null key总是存放在Entry[]数组的第一个元素，而且仅保留一个null key**。  
2) get  
首先也会根据hashCode的值定位到散列桶，并遍历存放在改桶中的链表。如果存在的该key则返回对应的value，**如果不存在则返回null**。（比较时调用key.equals)  具体比较：
(e.hash == hash && ((k = e.key) == key || key.equals(k)))）  
再散列rehash过程：   
Entry[]的长度一定后，随着map里面数据的越来越长，超过了加载因子，必须调整table的大小。每次将数组拓展到原来数据长度的两倍。这时，需要创建一张新表，将原表的映射到新表中。    
解决hash冲突的办法  
1.开放定址法（线性探测再散列，二次探测再散列，伪随机探测再散列）   
2.链地址法   
Java中hashmap的解决办法就是采用的链地址法。  
HashMap 的底层数组长度为何总是2的n次方？  
1. 发生碰撞的概率比较小，这样就会使得数据在table数组中分布较均匀，空间利用率较高，查询速度也较快；   
2. h&(length - 1) 就相当于对length取模，而且在速度、效率上比直接取模要快得多，即二者是等价不等效的，这是HashMap在速度和效率上的一个优化。  


http://blog.csdn.net/justloveyou_/article/details/62893086

## HashMap和HashTable的区别
HashMap和Hashtable都实现了Map接口，因此很多特性非常相似。但是，他们有以下不同点： 
1. HashMap和Hashtable的实现模板不同：虽然二者都实现了Map接口，但HashTable继承于Dictionary类，而HashMap是继承于AbstractMap。Dictionary是任何可将键映射到相应值的类的抽象父类，而AbstractMap是基于Map接口的骨干实现，它以最大限度地减少实现此接口所需的工作。   
2. HashMap和Hashtable对键值的限制不同：HashMap可以允许存在一个为null的key和任意个为null的value，但是HashTable中的key和value都不允许为null。  
3. HashMap和Hashtable的线程安全性不同：Hashtable的方法是同步的，实现线程安全的Map；而HashMap的方法不是同步的，是Map的非线程安全实现。  
4. HashMap和Hashtable的地位不同：在并发环境下，Hashtable虽然是线程安全的，但是我们一般不推荐使用它，因为有比它更高效、更好的选择ConcurrentHashMap；而单线程环境下，HashMap拥有比Hashtable更高的效率(Hashtable的操作都是同步的，导致效率低下)，所以更没必要选择它了。   
5. HashMap的**迭代器(Iterator)是fail-fast迭代器**，而Hashtable的**enumerator(列举)迭代器不是fail-fast的**。所以当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。这条同样也是Enumeration和Iterator的区别。一般认为Hashtable是一个遗留的类。如果你寻求在迭代的时候修改Map，你应该使用CocurrentHashMap

## ConcurrentHashMap
ConcurrentHashMap和Hashtable主要区别就是围绕着锁的粒度以及如何锁。  
Hashtable在竞争激烈的环境下表现效率低下的原因是一把锁锁住整张表，导致所有线程同时竞争一个锁。ConcurrentHashMap采用分段锁，每把锁锁住容器中的一个Segment。那么多线程访问容器里不同的Segment的数据时线程就不会存在竞争，从而有效提高并发访问效率。首先是将数据分层多个Segment存储，并为每个Segment分配一把锁，当一个线程范围其中一段数据时，其他线程可以访问其他段的数据。  
在理想状态下，ConcurrentHashMap 可以支持 16 个线程执行并发写操作（如果并发级别设为16），及任意数量线程的读操作。  
数据结构：  
ConcurrentHashMap本质上是一个Segment数组，而一个Segment实例又包含若干个桶，每个桶中都包含一条由若干个 HashEntry 对象链接起来的链表。Segment 类继承于 ReentrantLock 类，从而使得 Segment 对象能充当锁的角色。ConcurrentHashMap不同于HashMap，它既不允许key值为null，也不允许value值为null。  
Segment的定位：根据key的hash值的高n位就可以确定元素到底在哪一个Segment中。（并发级别为16的话n就是4）。  
总的来说，ConcurrentHashMap读操作不需要加锁的奥秘在于以下三点：  
1、用HashEntery对象的不变性来降低读操作对加锁的需求。  
2、用Volatile变量协调读写线程间的内存可见性；  
3、若读时发生指令重排序现象，则加锁重读；  

ConcurrentHashMap高效的并发机制通过以下三点保证：  
1. 通过锁分段技术保证并发环境下的写操作；
2. 通过 HashEntry的不变性、Volatile变量的内存可见性和加锁重读机制保证高效、安全的读操作；
3. 通过不加锁和加锁两种方案控制跨段操作的的安全性。（例如size方法先不加锁统计各个segment大小，如果统计过程中，容器的count发生变化，那再采用加锁的方式来统计）

ConcurrentHashMap的**弱一致性**主要是为了提升效率，也是一致性与效率之间的一种**权衡**。要成为强一致性，就得到处使用锁，甚至是全局锁，这就与Hashtable和同步的HashMap一样了。ConcurrentHashMap的弱一致性主要体现在以下几方面：  
get操作是弱一致的：get操作只能保证一定能看到已完成的put操作；  
clear操作是弱一致的：在清除完一个segments之后，正在清理下一个segments的时候，已经清理的segments可能又被加入了数据，因此clear返回的时候，ConcurrentHashMap中是可能存在数据的。  
ConcurrentHashMap中的迭代操作是弱一致的(未遍历的内容发生变化可能会反映出来)：在遍历过程中，如果已经遍历的数组上的内容变化了，迭代器不会抛出ConcurrentModificationException异常。如果未遍历的数组上的内容发生了变化，则有可能反映到迭代过程中

http://blog.csdn.net/justloveyou_/article/details/72783008

## LinkedHashMap
LinkedHashMap是HashMap的子类，所以LinkedHashMap自然会拥有HashMap的所有特性。HashMap是无序的，也就是说，迭代HashMap所得到的元素顺序并不是它们最初放置到HashMap的顺序。HashMap的这一缺点往往会造成诸多不便，因为在有些场景中，我们确需要用到一个可以保持插入顺序的Map。庆幸的是，JDK为我们解决了这个问题，它为HashMap提供了一个子类 —— LinkedHashMap。虽然LinkedHashMap增加了时间和空间上的开销，但是它通过维护一个额外的双向链表保证了迭代顺序。特别地，该迭代顺序可以是插入顺序，也可以是访问顺序。因此，根据链表中元素的顺序可以将LinkedHashMap分为：保持插入顺序的LinkedHashMap 和 保持访问顺序的LinkedHashMap，其中LinkedHashMap的默认实现是按插入顺序排序的。  
与HashMap相比，LinkedHashMap增加了两个属性用于保证迭代顺序，分别是 双向链表头结点header 和 标志位accessOrder (值为true时，表示按照访问顺序迭代；值为false时，表示按照插入顺序迭代)。   
LinkedHashMap采用的hash算法和HashMap相同，但是它重新定义了Entry。LinkedHashMap中的Entry增加了两个指针 before 和 after，它们分别用于维护双向链接列表。特别需要注意的是，next用于维护HashMap各个桶中Entry的连接顺序，before、after用于维护Entry插入的先后顺序的.   

## fail-fast机制
迭代器的快速失败行为仅用于检测程序错误，产生的原因就是程序对collection进行迭代时，某个线程对该collection在结构上对其做了修改，此时抛出ConcurrentModificationException，从而产生fail-fast。在遍历一个集合的时候，我们可以使用并发集合类来避免ConcurrentModificationException，比如使用CopyOnWriteArrayList，而不是ArrayList。

## Comparable和Comparator接口
Comparable & Comparator 都是用来实现集合中元素的比较、排序的，只是 Comparable 是在集合内部定义的方法实现的排序，Comparator 是在集合外部实现的排序，所以，如想实现排序，就需要在集合外定义 Comparator 接口的方法或在集合内实现 Comparable 接口的方法。  
1) Comparator位于包java.util下，而Comparable位于包java.lang下  
2)  **Comparable 是在集合内部定义的方法实现的排序,Comparator 是在集合外部实现的排序**.Comparable 是一个对象本身就已经支持自比较所需要实现的接口,自定义的类要在加入list容器中后能够排序，可以实现Comparable接口.在用Collections类的sort方法排序时，如果不指定Comparator，那么就以自然顺序排序.而 Comparator 是一个专用的比较器，当这个对象不支持自比较或者自比较函数不能满足你的要求时，你可以写一个比较器来完成两个对象之间大小的比较。**用 Comparator 是策略模式（strategy design pattern），就是不改变对象自身，而用一个策略对象（strategy object）来改变它的行为**。  
3) **Comparator定义了俩个方法，分别是int compare(T o1,T o2)和boolean equals(Object obj).Comparable接口只提供了int compareTo(T o)方法.**  
也就是说假如我定义了一个Person类，这个类实现了Comparable接口，那么当我实例化Person类的person1后，我想比较person1和一个现有的Person对象person2的大小时，我就可以这样来调用：person1.comparTo(person2),通过返回值就可以判断了；而此时如果你定义了一个PersonComparator（实现了Comparator接口）的话，那你就可以这样：PersonComparator    comparator=   new   PersonComparator();    
comparator.compare(person1,person2);。  

------------------------------------

## 快速失败(fail-fast)和安全失败(fail-safe)的区别
Iterator的fail-fast属性与当前的集合共同起作用，因此它不会受到集合中任何改动的影响。java.util包下面的所有的集合类都是快速失败的，而java.util.concurrent包下面的所有的类都是安全失败的。快速失败的迭代器会抛出ConcurrentModificationException异常，而安全失败的迭代器永远不会抛出这样的异常。

## 如何决定选用HashMap还是TreeMap
对于在**Map中插入、删除和定位元素这类操作，HashMap是最好的选择**。然而，**假如你需要对一个有序的key集合进行遍历，TreeMap是更好的选择**。基于你的collection的大小，也许向HashMap中添加元素会更快，将map换为TreeMap进行有序key的遍历。

## Enumeration 接口和 Iterator 接口的区别
Enumeration 速度是 Iterator 的2倍，**同时占用更少的内存**。
但是，Iterator远远比 Enumeration 安全，因为其他线程不能够修改正在被 iterator 遍历的集合里面的对象。
同时，Iterator 允许调用者删除底层集合里面的元素，这对 Enumeration 来说是不可能的。

## HashSet,TreeSet,LinkedHashSet 之间的区别
首先这三个类都是实现了Set接口，并且LinkedHashSet继承HashSet ，TreeSet实现了SortedSet。所以它们都拥有Set的基本特性，如：集合中的元素不能重复（这个需要和容器转载的类中的**equals()方法**有关）. 这三个容器所接受的类必须实现equals() 方法。
**HashSet**： 为查询速度所设计的，存入HashSet的元素必须定义hashCode()方法。内部是基于散列函数实现。
**TreeSet**： 实现了SortedSet（SortedSet的元素可以保证处于排序状态）,所以内部的元素是保持一定次序的（次序与元素实现的compareTo()方法有关），且底层是树状结构。使用它可以从Set中提取有序的序列。要求存入的元素必须实现Comparable接口,或则需要传入一个比较器Comparator。否则报ClassCastException。内部采用了**红黑树**实现。
**LinkedHashSet**： 具有HashSet的查询速度，且内部链表维护元素的顺序（按插入顺序排序）。元素必须实现hashCode(）方法。内部是由链表实现。

## HashMap ,LinkedHashMap ,TreeMap,WeakHashMap,ConcurrentHashMap,IdentityHashMap的区别
**Map** 中任何键的比较都是通过equals进行比较的，所以如果键用于映射的话需要由恰当的hashCode() 方法。如果键值用于TreeMap，它必须实现Comparable。
**HashMap** 基于散列表来的实现，即使用hashCode()进行快速查询元素的位置，显著提高性能。插入和查询“键值对”的开销是固定的。可以通过设置容量和负载因子，以调整容器的性能。
**LinkedHashMap**,  类似于HashMap,但是迭代遍历它时，取得“键值对”的顺序是其插入的次序，只比HashMap慢一点。而在迭代访问时反而更快，**因为它使用链表维护内部次序**。此外可以在构造器中设定LinkedHashMap，使之采用基于访问的最近最少使用(LRU)算法。于是没有被访问过的元素就会出现在队列前面。对于需要定期清理元素以节省空间的程序员来说，此功能使得程序员很容易得以实现。被访问过的键值会放在链表尾部。
**TreeMap**, 是基于**红黑树**的实现。实现了SortedMap，SortedMap 可以确保键处于排序状态。所以查看“键”和“键值对”时，所有得到的结果都是经过排序的，次序由Comparable或Comparator决定。SortedMap拥有其他额外的功能，如：Comparator comparator()返回当前Map使用的Comparator或者null. T firstKey() 返回Map的第一个键，T lastKey() 返回最后一个键。SortedMap headMap(toKey),生成一个键小于toKey的Map子集。SortedMap tailMap(fromKey) 也是生成一个子集。TreeMap是唯一的带有subMap()方法的Map,它可以返回一个子树。
**WeakHashMap**  表示**弱键映射**，允许释放映射所指向的对象。这是为了解决某类特殊问题而设计的，如果映射之外没有引用指向某个“键”，则“键”可以被垃圾收集器回收。
**ConcurrentHashMap** 一种线程安全的Map,它不涉及同步加锁。
**IdentityHashMap** 使用==代替equals() 对“键”进行比较的散列映射。专为解决特殊问题而设计。
**HashTable** (已经摒弃了)

## Map接口提供了哪些不同的集合视图
Map接口提供三个集合视图：
（1）**Set keyset()**：**返回map中包含的所有key的一个Set视图**。**集合是受map支持的，map的变化会在集合中反映出来，反之亦然**。当一个迭代器正在遍历一个集合时，若map被修改了（除迭代器自身的移除操作以外），迭代器的结果会变为未定义。集合支持通过**Iterator**的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。
（2）**Collection values()**：**返回一个map中包含的所有value的一个Collection视图**。这个collection受map支持的，map的变化会在collection中反映出来，反之亦然。当一个迭代器正在遍历一个collection时，若map被修改了（除迭代器自身的移除操作以外），迭代器的结果会变为**未定义**。集合支持通过Iterator的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。
（3）Set< Map.Entry < K,V > > entrySet()：**返回一个map钟包含的所有映射的一个集合视图**。这个集合受map支持的，map的变化会在collection中反映出来，反之亦然。当一个迭代器正在遍历一个集合时，若map被修改了（除迭代器自身的移除操作，以及对迭代器返回的entry进行setValue外），**迭代器的结果会变为未定义**。集合支持通过Iterator的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。


## Iterator是什么
Iterator接口提供遍历任何Collection的接口。我们可以从一个Collection中使用迭代器方法来获取迭代器实例。迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者在迭代过程中移除元素。

## Iterator和ListIterator的区别是什么
下面列出了他们的区别：
Iterator 可用来遍历 Set 和 List 集合，但是 ListIterator 只能用来遍历 List 。
Iterator 对集合只能是前向遍历，ListIterator 既可以前向也可以后向。
ListIterator 实现了 Iterator 接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。 

## 为何Iterator接口没有具体的实现
Iterator接口定义了遍历集合的方法，但它的实现则是集合实现类的责任。每个能够返回用于遍历的Iterator的集合类都有它自己的Iterator实现内部类。
这就允许集合类去选择迭代器是fail-fast还是fail-safe的。比如，ArrayList迭代器是fail-fast的，而CopyOnWriteArrayList迭代器是fail-safe的

## EnumSet是什么
java.util.EnumSet是使用**枚举类型**的集合实现。当集合创建时，枚举集合中的所有元素必须来自单个指定的枚举类型，可以是显示的或隐示的。EnumSet是不同步的，不允许值为null的元素。它也提供了一些有用的方法，比如copyOf(Collection c)、of(E first,E…rest)和complementOf(EnumSet s)。

## 哪些集合类是线程安全的
Vector、HashTable、Properties和Stack是同步类，所以它们是线程安全的，可以在多线程环境下使用。Java1.5并发API包括一些集合类，允许迭代时修改，因为它们都工作在集合的克隆上，所以它们在多线程环境中是安全的。

## BockingQueue是什么
Java.util.concurrent.BlockingQueue是一个队列，在进行检索或移除一个元素的时候，它会等待队列变为非空；当在添加一个元素时，它会等待队列中的可用空间。BlockingQueue接口是Java集合框架的一部分，主要用于实现生产者-消费者模式。我们不需要担心等待生产者有可用的空间，或消费者有可用的对象，因为它都在BlockingQueue的实现类中被处理了。Java提供了集中BlockingQueue的实现，比如ArrayBlockingQueue、LinkedBlockingQueue、PriorityBlockingQueue,、SynchronousQueue等。

## 我们如何对一组对象进行排序
 如果我们需要对一个对象数组进行排序，我们可以使用Arrays.sort()方法。如果我们需要排序一个对象列表，我们可以使用Collections.sort()方法。两个类都有用于自然排序（使用Comparable）或基于标准的排序（使用Comparator）的重载方法sort()。Collections内部使用数组排序方法，所有它们两者都有相同的性能，只是Collections需要花时间将列表转换为数组。

## Java集合框架是什么？说出一些集合框架的优点？
每种编程语言中都有集合，最初的Java版本包含几种集合类：Vector、Stack、HashTable和Array。随着集合的广泛使用，Java1.2提出了囊括所有集合接口、实现和算法的集合框架。在保证线程安全的情况下使用泛型和并发集合类，Java已经经历了很久。它还包括在Java并发包中，阻塞接口以及它们的实现。集合框架的部分优点如下：
（1）使用核心集合类降低开发成本，而非实现我们自己的集合类。
（2）随着使用经过严格测试的集合框架类，代码质量会得到提高。
（3）通过使用JDK附带的集合类，可以降低代码维护成本。
（4）复用性和可操作性。

## 集合框架中的泛型有什么优点
Java1.5引入了泛型，所有的集合接口和实现都大量地使用它。泛型允许我们为集合提供一个可以容纳的对象类型，因此，如果你添加其它类型的任何元素，它会在编译时报错。这避免了在运行时出现ClassCastException，因为你将会在编译时得到报错信息。泛型也使得代码整洁，我们不需要使用显式转换和instanceOf操作符。它也给运行时带来好处，因为不会产生类型检查的字节码指令。

## 为何Map接口不继承Collection接口
尽管Map接口和它的实现也是集合框架的一部分，但Map不是集合，集合也不是Map。因此，Map继承Collection毫无意义，反之亦然。
如果Map继承Collection接口，那么元素去哪儿？Map包含key-value对，它提供抽取key或value列表集合的方法，但是它不适合“一组对象”规范。

## 为什么集合类没有实现Cloneable和Serializable接口
Collection接口指定一组对象，对象即为它的元素。**如何维护这些元素由Collection的具体实现决定**。例如，一些如List的Collection实现允许重复的元素，而其它的如Set就不允许。**很多Collection实现有一个公有的clone方法。然而，把它放到集合的所有实现中也是没有意义的。这是因为Collection是一个抽象表现。重要的是实现**。
**当与具体实现打交道的时候，克隆或序列化的语义和含义才发挥作用**。所以，具体实现应该决定如何对它进行克隆或序列化，或它是否可以被克隆或序列化。
**在所有的实现中授权克隆和序列化，最终导致更少的灵活性和更多的限制**。特定的实现应该决定它是否可以被克隆和序列化。

## 与Java集合框架相关的有哪些最好的实践
(1)根据需要选择正确的集合类型。比如，如果指定了大小，我们会选用Array而非ArrayList。如果我们想根据插入顺序遍历一个Map，我们需要使用TreeMap。如果我们不想重复，我们应该使用Set。如果涉及到堆栈，队列等操作，应该考虑用List，对于需要快速插入，删除元素，应该使用LinkedList，如果需要快速随机访问元素，应该使用ArrayList。
(2)如果程序在单线程环境中，或者访问仅仅在一个线程中进行，考虑非同步的类，其效率较高，如果多个线程可能同时操作一个类，应该使用同步的类。
(3)要特别注意对哈希表的操作，作为key的对象要正确复写equals和hashCode方法。使用JDK提供的不可变类作为Map的key，可以避免自己实现hashCode()和equals()。
性。
(4)尽量返回接口而非实际的类型，如返回List而非ArrayList，这样如果以后需要将ArrayList换成LinkedList时，客户端代码不用改变。这就是针对抽象编程。
(5)尽可能使用Collections工具类，或者获取只读、同步或空的集合，而非编写自己的实现。它将会提供代码重用性，它有着更好的稳定性和可维护
(6)一些集合类允许指定**初始容量**，所以如果我们能够估计到存储元素的数量，我们可以使用它，就避免了重新哈希或大小调整。
(7)基于接口编程，而非基于实现编程，它允许我们后来轻易地改变实现。
(8)总是使用类型安全的泛型，避免在运行时出现ClassCastException。

## ArrayList 是否支持序列化
支持，
实现了　serlializable接口，并实现了以下两个方法：
```
　　 　private void writeObject(java.io.ObjectOutputStream s)
	throws java.io.IOException{
	// Write out element count, and any hidden stuff
	int expectedModCount = modCount;
	s.defaultWriteObject();

	// Write out size as capacity for behavioural compatibility with clone()
	s.writeInt(size);

	// Write out all elements in the proper order.
	for (int i=0; i<size; i++) {
	    s.writeObject(elementData[i]);
	}

	if (modCount != expectedModCount) {
	    throw new ConcurrentModificationException();
	}
    }

    /**
     * Reconstitute the <tt>ArrayList</tt> instance from a stream (that is,
     * deserialize it).
     */
    private void readObject(java.io.ObjectInputStream s)
	throws java.io.IOException, ClassNotFoundException {
	elementData = EMPTY_ELEMENTDATA;

	// Read in size, and any hidden stuff
	s.defaultReadObject();

	// Read in capacity
	s.readInt(); // ignored

	if (size > 0) {
	    // be like clone(), allocate array based upon size not capacity
	    ensureCapacityInternal(size);

	    Object[] a = elementData;
	    // Read in all elements in the proper order.
	    for (int i=0; i<size; i++) {
	        a[i] = s.readObject();
	    }
	}
    }
```

ArrayList
```
 public Object clone() {
	try {
	    ArrayList<?> v = (ArrayList<?>) super.clone();
	    v.elementData = Arrays.copyOf(elementData, size);
	    v.modCount = 0;
	    return v;
	} catch (CloneNotSupportedException e) {
	    // this shouldn't happen, since we are Cloneable
	    throw new InternalError(e);
	}
    }
```
## 为什么集合类没有实现 Cloneable 和 Serializable 接口
## Comparable 和Comparator 接口



