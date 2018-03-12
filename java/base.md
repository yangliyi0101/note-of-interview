## 1.Java 语言的优点
面向对象,平台无关,内存管理,安全性,多线程,Java 是解释型的（java代码编译后不能直接运行，它是解释运行在JVM上的）。

### 2.Java 和 C++的区别
1. **多重继承**(java接口多重,类不支持,C++支持)   
2. **自动内存管理**：java有完善的垃圾回收机制。在C++中可以直接手动释放内存，Java的垃圾由虚拟机回收，不需要程序员操作。  
3. **预处理功能**：java不支持预处理功能。c／c++在编译过程中都有一个预编译阶段，即预处理器，预处理器为开发人员提供了方便但增加了编译的复杂性。    
4. **goto语句**(java不支持)  
5. **引用与指针**。**Java中的引用和C++中的指针在概念上是相似的，他们都是存放的对象在内存中的地址值**，只是在Java中，引用丧失了部分灵活性，比如Java中的引用不能像C++中的指针那样进行**加减运算**。 

## 3.值传递和引用传递
变量被值传递，意味着传递了**变量的一个副本**。因此，就算是改变了变量副本，也不会影响源对象的值。  
对象被引用传递，意味着传递的并不是实际的对象，而是**对象的引用**。因此，外部对引用对象所做的改变会反映到所有的对象上。
java本质上还是值传递，如方法调用的时候传入一个对象引用进去，在方法栈中会构建一个副本和该引用变量值相同指向同一个地址（所以看上去是引用传递，其实还是值传递）。如果改变引用的值不会对改变传入的引用的值。

## 4.静态变量和实例变量的区别
**在语法定义上的区别**：静态变量前要加 static 关键字，而实例变量前则不加。  
**在程序运行时的区别**：实例变量属于某个对象的属性，必须创建了实例对象，其中的实例变量才会被分配空间，才能使用这个实例变量。静态变量不属于某个实例对象，而是属于类，所以也称为类变量，只要程序加载了类的字节码，不用创建任何实例对象，静态变量就会被分配空间，静态变量就可以被使用了。总之**，实例变量必须创建对象后才可以通过这个对象来使用，静态变量则可以直接使用类名来引用**。

## 5.JDK 包.
JDK 常用的 package  
**java.lang**：这个是系统的基础类，比如 String 等都是这里面的，这个 package 是唯一一个可以不用 import 就可以使用的 Package  
**java.io**: 这里面是所有输入输出有关的类，比如文件操作等  
**java.net**: 这里面是与网络有关的类，比如 URL,URLConnection 等。  
**java.util**: 这个是系统辅助类，特别是集合类 Collection,List,Map 等。  
**java.sql**: 这个是数据库操作的类，Connection, Statememt，ResultSet 等  

## 6.JDK, JRE 和 JVM 的区别
JDK,JRE和JVM 是 Java 编程语言的核心概念。尽管它们看起来差不多，作为程序员我们也不怎么关心这些概念，但是它们是针对特定目的的不同的产品。这是一道常见的 Java 面试题，而本文则会一一解释这些概念并给出它们之间的区别。  
**1）Java 开发工具包 (JDK)**  
Java 开发工具包是 Java 环境的**核心组件，并提供编译、调试和运行一个 Java 程序所需的所有工具，可执行文件和二进制文件**。包括java基础jar包、虚拟机、javac等可执行文件等。JDK 是一个平台特定的软件，有针对 Windows，Mac 和 Unix 系统的不同的安装包。可以说 JDK 是 JRE 的超集，它包含了 JRE 的 Java 编译器，调试器和核心类。  
**2）Java 虚拟机(JVM)**  
JVM 是 Java 编程语言的核心。当我们运行一个程序时，JVM 负责将字节码转换为特定机器代码。JVM 也是平台特定的，并提供核心的 Java 方法，例如内存管理、垃圾回收和安全机制等。JVM 是可定制化的，我们可以通过 Java 选项(java options)定制它，比如配置 JVM 内存的上下界。JVM 之所以被称为虚拟的是因为它提供了一个不依赖于底层操作系统和机器硬件的接口。这种独立于硬件和操作系统的特性正是 Java 程序可以一次编写多处执行的原因。  
**3）Java 运行时环境(JRE)**  
JRE 是 JVM 的实施实现，它提供了运行 Java 程序的平台。**JRE 包含了 JVM、Java 二进制文件和其它成功执行程序的类文件**。JRE 不包含任何像 Java 编译器、调试器之类的开发工具。如果你只是想要执行 Java 程序，你只需安装 JRE 即可，没有安装 JDK 的必要。

**JDK, JRE 和 JVM 的区别**  
JDK 是用于开发的而 JRE 是用于运行 Java 程序的。  
JDK 和 JRE 都包含了 JVM，从而使得我们可以运行 Java 程序。  
JVM 是 Java 编程语言的核心并且具有平台独立性。  
**即时编译器(JIT)**  
有时我们会听到 JIT 这个概念，并说它是 JVM 的一部分，这让我们很困惑。**JIT 是 JVM 的一部分**，它可以在同一时间编译类似的字节码来**优化**将字节码转换为机器特定语言的过程相似的字节码，从而将**优化字节码转换为机器特定语言的过程**，这样减少转换过程所需要花费的时间。  

## Java 中的 final关键字用法
(1)修饰类：表示该类不能被继承；  
(2)修饰方法：表示方法不能被覆盖（重写）；  
(3)修饰变量：表示变量只能一次赋值以后值不能被修改（常量）。

## assert
assertion(断言)在软件开发中是一种常用的调试方式，很多开发语言中都支持这种机制。一般来说，assertion **用于保证程序最基本、关键的正确性**。assertion 检查通常在开发和测试时开启。为了提高性能，在软件发布后， assertion 检查通常是关闭的。在实现中，断言是一个包含布尔表达式的语句，在执行这个语句时假定该表达式为 true；**如果表达式计算为 false，那么系统会报告一个 AssertionError**。  

断言用于调试目的：  

		assert(a > 0); // throws an AssertionError if a <= 0

断言可以有两种形式：  

		assert Expression1;
		assert Expression1 : Expression2 ;

Expression1 应该总是产生一个布尔值。  
Expression2 可以是得出一个值的任意表达式；这个值用于生成显示更多调试信息的**字符串消息**  
断言在默认情况下是禁用的，要在编译时启用断言，需使用 source 1.4 标记：  

		javac -source 1.4 Test.java

要在运行时启用断言，可使用 -enableassertions 或者 -ea 标记。  
要在运行时选择禁用断言，可使用 -da 或者 -disableassertions 标记。  

## final, finally, finalize 的区别?
**final**：修饰符（关键字）有三种用法：如果一个类被声明为final，意味着它不能再派生出新的子类，即不能被继承，因此它和 abstract 是反义词。将变量声明为 final，可以保证它们在使用中不被改变，被声明为 final 的变量必须在声明时给定初值，而在以后的引用中只能读取不可修改。被声明为 final 的方法也同样只能使用，不能在子类中被重写。   
**finally**：通常放在 try…catch 的后面构造总是执行代码块，这就意味着程序无论正常执行还是发生异常，这里的代码只要 JVM 不关闭都能执行，可以将释放外部资源的代码写在 finally 块中。  
**finalize**：Object 类中定义的方法，Java 中允许使用 finalize() 方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在销毁对象时调用的，通过重写finalize() 方法可以整理系统资源或者执行其他清理工作。

## & 和 &&
& 和 && 都可以用作逻辑与的运算符，表示逻辑与（and），当运算符两边的表达式的结果都为 true 时，整个运算结果才为 true，否则，只要有一方为 false，则结果为 false。  
**&& 还具有短路的功能**，即如果第一个表达式为 false，则不再计算第二个表达式.    
**& 还可以用作位运算符**，当 & 操作符两边的表达式不是 boolean 类型时，& 表示按位与操作.

## 用最有效率的方法算出 2 乘以 8 等於几
2 << 3，因为将一个数左移 n 位，就相当于乘以了 2 的 n 次方，那么，一个数乘以 8 只要将其左移 3 位即可，而位运算 cpu 直接支持的，效率最高，所以，2 乘以 8 等於几的最效率的方法是 2 << 3 。

## char 型变量
**char 类型可以存储一个中文汉字**，因为 **Java 中使用的编码是 Unicode**（不选择任何特定的编码，直接使用字符在字符集中的编号，这是统一的唯一方法），一个 char 类型占 2 个字节（16bit），所以放一个中文是没问题的。  
补充：使用 Unicode 意味着字符在 JVM 内部和外部有不同的表现形式，在 JVM 内部都是 Unicode，当这个字符被从 JVM 内部转移到外部时（例如存入文件系统中），需要进行编码转换。所以 Java 中有字节流和字符流，以及在字符流和字节流之间进行转换的转换流，如 InputStreamReader 和 OutputStreamReader.

## String 和StringBuilder、StringBuffer 的区别
答：Java 平台提供了两种类型的字符串：String 和StringBuffer / StringBuilder，它们可以储存和操作字符串。其中 String 是**只读**字符串，也就意味着 String 引用的字符串内容是不能被改变的。而 StringBuffer 和 StringBuilder 类表示的字符串对象可以直接进行修改。StringBuilder 是 JDK 1.5 中引入的，它和 StringBuffer 的方法完全相同，区别在于它是在单线程环境下使用的，因为它的所有方法都没有被 synchronized 修饰，因此它的效率也比 StringBuffer 略高。  
有一个面试题问：有没有哪种情况用 + 做字符串连接比调用 StringBuffer / StringBuilder 对象的 append 方法性能更好？  
如果连接后得到的字符串在静态存储区中是早已存在的，那么用+做字符串连接是优于 StringBuffer / StringBuilder 的 append 方法的。  
如果在编写代码的过程中大量使用+进行字符串评价还是会对性能造成比较大的影响，但是使用的个数在1000以下还是可以接受的，大于10000的话，执行时间将可能超过1s，会对性能产生较大影响。如果有大量需要进行字符串拼接的操作，最好还是使用StringBuffer或StringBuilder进行。  

## String、StringBuffer与StringBuilder的区别
1. String是字符串常量，是不可变类。如果要操作少量的数据用  
2. StringBuffer是字符串变量，是线程安全的。多线程操作字符串缓冲区 下操作大量数据  
3. StringBuilder是字符串变量，非线程安全。单线程操作字符串缓冲区 下操作大量数据  
速度：StringBuilder >  StringBuffer  >  String  
String 对象的字符串拼接其实是被 JVM 解释成了 StringBuffer 对象的拼接，所以这些时候 String 对象的速度并不会比 StringBuffer 对象慢，而特别是以下的字符串对象生成中，String 效率是远要比 StringBuffer 快的：  
String S1 = “This is only a” + “ simple” + “ test”;  
StringBuffer Sb = new StringBuilder(“This is only a”).append(“simple”).append(“ test”);  
这里JVM做了优化String S1 = “This is only a” + “ simple” + “ test”;相当于String S1 = “This is only a simple test”;  
参见：http://www.findspace.name/easycoding/1090

## 为什么String要设计成不可变的
在Java中将String设计成不可变的是综合考虑到各种因素的结果,如内存,同步,数据结构以及安全等方面的考虑.  
1. 字符串常量池的需要.  
 字符串池的实现可以在运行时节约很多heap空间，因为不同的字符串变量都指向池中的同一个字符串。但如果字符串是可变的，那么String interning将不能实现(译者注：String interning是指对不同的字符串仅仅只保存一个，即不会保存多个相同的字符串。)，因为这样的话，如果变量改变了它的值，那么其它指向这个值的变量的值也会一起改变。
2. 线程安全考虑。  
同一个字符串实例可以被多个线程共享。这样便不用因为线程安全问题而使用同步。字符串自己便是线程安全的。    
3. 类加载器要用到字符串，不可变性提供了安全性，以便正确的类被加载。譬如你想加载java.sql.Connection类，而这个值被改成了myhacked.Connection，那么会对你的数据库造成不可知的破坏。  
4. 支持hash映射和缓存。  
因为字符串是不可变的，所以在它创建的时候hashcode就被缓存了，不需要重新计算。这就使得字符串很适合作为Map中的键，字符串的处理速度要快过其它的键对象。这就是HashMap中的键往往都使用字符串。

## 什么是 Java 序列化，如何实现 Java 序列化
序列化就是一种用来处理对象流的机制，**所谓对象流也就是将对象的内容进行流化**。**可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间**。序列化是为了解决在对对象流进行读写操作时所引发的问题。   

序列化的实现：将需要被序列化的类实现 Serializable 接口，该接口没有需要实现的方法， implements Serializable 只是为了**标注该对象是可被序列化**的，然后使用一个输出流(如：FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream对象的writeObject(Object obj)方法就可以将参数为obj的对象写出(即保存其状态)，要恢复的话则用输入流。

## Switch能否用String做参数？
Java 1.7之前不可以，java 1.7后String可以作为参数。  
整型（byte，short int，int，long int），枚举类型，boolean，字符型(char),字符串都可以，唯独浮点型不可以

## equals与==的区别
**关系操作符 ==：**  
若操作数的类型是基本数据类型，则该关系操作符判断的是左右两边操作数的值是否相等  
若操作数的类型是引用数据类型，则该关系操作符判断的是左右两边操作数的内存地址是否相同。也就是说，若此时返回true,则该操作符作用的一定是同一个对象。  
**equals方法：**  
来源：equals方法是基类Object中的实例方法，因此对所有继承于Object的类都会有该方法。 

		 public boolean equals(Object obj) {
		 	return (this == obj);
		 }
很显然，在Object类中，equals方法是用来比较两个对象的引用是否相等，即是否指向同一个对象。  
但在String中重写了equals方法，重写的方法里判断分三步：先比较引用是否相同(是否为同一对象)，再判断类型是否一致（是否为同一类型），最后 比较内容是否一致。Java 中所有内置的类的 equals 方法的实现步骤均是如此，特别是诸如 Integer，Double 等包装器类。   
equals 重写原则：  
对称性： 如果x.equals(y)返回是“true”，那么y.equals(x)也应该返回是“true” ；  
自反性： x.equals(x)必须返回是“true” ；  
类推性： 如果x.equals(y)返回是“true”，而且y.equals(z)返回是“true”，那么z.equals(x)也应该返回是“true” ；  
一致性： 如果x.equals(y)返回是“true”，只要x和y内容一直不变，不管你重复x.equals(y)多少次，返回都是“true” ；   
因此，**对于 equals 方法：其本意 是 比较两个对象的 content 是否相同**  

hashCode 方法是基类Object中的实例native方法，因此对所有继承于Object的类都会有该方法。  
	 
	 public native int hashCode();
在 Java 中，由 Object 类定义的 hashCode 方法会针对不同的对象返回不同的整数。  
如果 x.equals(y) 返回 “true”，那么 x 和 y 的 hashCode() 必须相等 ；   
如果 x 和 y 的 hashCode() 不相等，那么 x.equals(y) 一定返回 “false” ；  

小结：  
hashcode是系统用来快速检索对象而使用（set集合中添加数据先判断hashcode，再equals）     
重写equals方法和hashcode方法时，equals方法中用到的成员变量也必定会在hashcode方法中用到,只不过前者作为比较项，后者作为生成摘要的信息项，本质上所用到的数据是一样的，从而保证二者的一致性（例如String中 重写了hashcode方法 s[0]*31^n-1 + s[1]*31^n-2 + ...... n是字符串的长度）   

## Object有哪些公用方法
protected Object clone()创建并返回此对象的一个副本。 
public boolean equals(Object obj)指示其他某个对象是否与此对象“相等”。   
protected void finalize()当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。   
public final native Class< ? > getClass() 返回此 Object 的运行时类。  
public int hashCode()返回该对象的哈希码值。   
public String toString()返回该对象的字符串表示。   
public void notify()唤醒在此对象监视器上等待的单个线程。   
public void notifyAll()唤醒在此对象监视器上等待的所有线程。   
public void wait()在其他线程调用此对象的 notify() 方法或 notifyAll() 方法前，导致当前线程等待。   
public void wait(long timeout)在其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量前，导致当前线程等待。   
public void wait(long timeout, int nanos)在其他线程调用此对象的 notify() 方法或  

注意：
1。 如果一个类实现了Cloneable,Object的clone方法就返回该对象的逐域拷贝，否则就会抛出CloneNotSupportException异常。java.lang.Cloneable 是一个标示性接口，不包含任何方法（详见《effective java》 p46）。   
2. 覆盖equals（）时总要覆盖hashCode（）（详见《effective java》p39）   
在每个覆盖equals方法的类中也必须覆盖hashCode方法，如果不这样做就会导致Object.hashCode的通用约定---**相等的对象必须具有相等的散列码的约定**，从而导致该类无法集合所有基于散列集合一起工作，这样的集合有HashMap,HashSet和HashTable。HashMap等使用Key对象的hashCode()和equals()方法去决定key-value对的索引。如果将equals相等的对象认为是同一个对象的话，那么put方法将对象放在一个散列桶，而get方法可能从另一个散列桶获取该对象，因为这两个方法传入的对象虽然equals相同，但hashCode可能不同，而hashMap根据hashCode去定位散列桶位置导致出现在不同的散列桶中。  
 Hashcode的作用。  
1. hashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，hashCode是用来在散列存储结构中确定对象的存储地址的；  
2. 比较对象是否相同。    

## try catch finally，try里有return，finally还执行么？
1)不管有木有出现异常，finally块中代码都会执行   
2)当try和catch中有return时，finally仍然会执行   
3)finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，**不管finally中的代码怎么样，返回的值都不会改变**，任然是之前保存的值），所以函数返回值是在finally执行前确定的   
4)finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值  

##  Exception与Error包结构
在Java中，Throwable是所有异常类型的根类，Throwable有两个直接子类：Exception 和 Error。Error类是error类型异常的父类，Exception类是exception类型异常的父类，RuntimeException类是所有运行时异常的父类，RuntimeException以外的并且继承Exception的类是非运行时异常。  
常见的RuntimeException包括NullPointerException、IndexOutOfBoundsException。  
常见的非RuntimeException包括IOException、SQLException等。  
**Error** **是程序无法处理的错误**，表示运行应用程序中**较严重问题**。这些错误大部分与代码编写者执行的操作无关，而与代码运行时的 JVM 、资源等有关。例如，Java虚拟机运行错误（Virtual MachineError），当 JVM 不再有继续执行操作所需的内存资源时，将出现 OutOfMemoryError。这些异常发生时，Java虚拟机（JVM）一般会选择线程终止。这些错误是不可查的，并且它们在应用程序的控制和处理能力之外。在 Java 中，错误通过Error的子类描述。  
**Exception** 通常是Java程序员所关心的，其在Java类库、用户方法及运行时故障中都可能抛出。它由两个分支组成： **运行时异常**（派生于 RuntimeException 的异常）和**非运行时异常** 。划分这两种异常的规则是：由**程序错误**（一般是逻辑错误，如错误的**类型转换**、**数组越界**等，应该避免）导致的异常属于RuntimeException；而程序本身没有问题，但由于诸如**I/O这类错误**（eg：试图打开一个不存在的文件）导致的异常就属于非运行时异常。  
Java的异常(包括Exception和Error)通常可分为 **受检查的异常**（checked exceptions） 和 **不受检查的异常**（unchecked exceptions） 两种类型。**不受检查异常是编译器不要求强制处理的异常**，包括运行时异常（RuntimeException与其子类）和错误（Error）。**受检查异常是编译器要求必须处理的异常**。这里所指的处理方式有两种： 捕获并处理异常 和声明抛出异常 。也就是说，当程序中可能出现这类异常，要么用 try-catch 语句捕获它，要么用 throws 子句声明抛出它，否则编译不会通过。  

解读：RuntimeException所表示的是软件开发人员没有正确地编写代码所导致的问题，如数组访问越界等。**如果程序出现RuntimeException异常，那么一定是程序员的问题**。而非运行时异常所表示的并不是代码本身的不足所导致的非正常状态，而是一系列应用本身也无法控制的情况。例如一个应用在尝试打开一个文件并写入的时候，该文件已经被另外一个应用打开从而无法写入。对于这些情况，**Java通过Checked Exception来强制软件开发人员在编写代码的时候就考虑对这些无法避免的情况的处理**，从而提高代码质量。而Error则是一系列很难通过程序解决的问题。这些问题基本上是无法恢复的，例如内存空间不足等。在这种情况下，我们基本无法使得程序重新回到正常轨道上。  
**如何正确使用Checked Exception**。首先，Checked Exception应当只在异常情况对于API以及API的使用者都无法避免的情况下被使用。例如在打开一个文件的时候，API以及API的使用者都没有办法保证该文件一定存在。反过来，在通过索引访问数据的时候，如果API的使用者对参数index传入的是-1，那么这就是一个代码上的错误，是完全可以避免的。因此对于index参数值不对的情况，我们应该使用Unchecked Exception。其次，Checked Exception不应该被广泛调用的API所抛出。这一方面是基于代码整洁性的考虑，另一方面则是因为Checked Exception本身的实际意义是API以及API的使用者都无法避免的情况。如果一个应用有太多处这种“无法避免的异常”，那么这个程序是否拥有足够的质量也是一个很值得考虑的问题。而就API提供者而言，在一个主要的被广泛使用的功能上抛出这种异常，也是对其自身API的一种否定。再次，一个Checked Exception应该有明确的意义。这种明确意义的标准则是需要让API使用者能够看到这个Checked Exception所对应的异常类，该异常类所包含的各个域，并阅读相应的API文档以后就能够了解到底哪里出现了问题，进而向用户提供准确的有关该异常的解释。

## 运行时异常与受检异常有何异同？
答：异常表示程序运行过程中可能出现的非正常状态，运行时异常表示虚拟机的通常操作中可能遇到的异常，是一种常见运行错误，只要程序设计得没有问题通常就不会发生。受检异常跟程序运行的**上下文环境有关，即使程序设计无误，仍然可能因使用的问题而引发**。Java编译器要求方法必须声明抛出可能发生的受检异常，但是并不要求必须声明抛出未被捕获的运行时异常。异常和继承一样，是面向对象程序设计中经常被滥用的东西，神作《Effective Java》中对异常的使用给出了以下指导原则：  
    **不要将异常处理用于正常的控制流（设计良好的API不应该强迫它的调用者为了正常的控制流而使用异常）**    
    **对可以恢复的情况使用受检异常，对编程错误使用运行时异常**   
    **避免不必要的使用受检异常（可以通过一些状态检测手段来避免异常的发生）**  
    **优先使用标准的异常**   
    **每个方法抛出的异常都要有文档**  
    **保持异常的原子性**  
    **不要在 catch 中忽略掉捕获到的异常**  

## 常见的runtime exception
1、NullPointerException  空指针引用异常  
2、ClassCastException - 类型强制转换异常。  
3、IllegalArgumentException - 传递非法参数异常。  
4、IndexOutOfBoundsException - 下标越界异常  
5、UnsupportedOperationException - 不支持的操作异常  
6、ArithmeticException - 算术运算异常  
7、ArrayStoreException - 向数组中存放与声明类型不兼容对象异常  
8、NegativeArraySizeException - 创建一个大小为负数的数组错误异常  
9、NumberFormatException - 数字格式异常  
10、SecurityException - 安全异常  

## throw和throws有什么区别
throw关键字用来在程序中明确的抛出异常，相反，throws语句用来表明方法不能处理的异常。每一个方法都必须要指定哪些异常不能处理，所以方法的调用者才能够确保处理可能发生的异常，多个异常是用逗号分隔的。


## 访问控制权限
	
	       本类		同包子类	同包非子类	非同包子类   非同包非子类
	public	Yes	 		Yes			Yes			Yes         Yes
	protected	Yes	   Yes			Yes			Yes         No
	default（缺省）YesYes			Yes			No          No
	private	Yes		No			No			No          No


## 反射
反射可以实现在运行时可以知道任意一个类的属性和方法。  
为什么要用反射机制？直接创建对象不就可以了吗，这就涉及到了动态与静态的概念。静态编译：在编译时确定类型，绑定对象,即通过。动态编译：运行时确定类型，绑定对象。动态编译最大限度发挥了java的灵活性，体现了多态的应用，有以降低类之间的藕合性。  
优点：可以实现动态创建对象和编译，体现出很大的灵活性，特别是在J2EE的开发中它的灵活性就表现的十分明显。比如，一个大型的软件，不可能一次就把把它设计的很完美，当这个程序编译后，发布了，当发现需要更新某些功能时，我们不可能要用户把以前的卸载，再重新安装新的版本，假如这样的话，这个软件肯定是没有多少人用的。采用静态的话，需要把整个程序重新编译一次才可以实现功能的更新，而采用反射机制的话，它就可以不用卸载，只需要在运行时才动态的创建和编译，就可以实现该功能。  
缺点：对性能有影响。使用反射基本上是一种解释操作，我们可以告诉JVM，我们希望做什么并且它满足我们的要求。这类操作总是慢于只直接执行相同的操作。  

## 泛型
有许多原因促成了泛型的出现，而最重要的一个原因，就是为了**更安全友好的使用容器类**: 用来指定容器要持有什么类型的对象，而且由编译器来保证类型的正确性。  
在 Java 加入泛型特性前，ArrayList 只维护一个 Object 类型的数组： 当获取一个值时，必须进行强制类型转换：该 ArrayList 没有错误检查，即可以向数组中添加任何类型的对象：  

泛型 = 编译时的类型检查 + 编译时的类型擦除(为了兼容之前的jdk版本) + 运行时的自动类型转换（不需要再强制类型转换了）。  

泛型的限定通配符有两种，一种是<? extends T>它通过确保类型必须是T的子类来设定类型的上界，另一种是<? super T>它通过确保类型必须是T的父类来设定类型的下界。  
<?>表 示了非限定通配符，因为<?>可以用任意类型来替代。

