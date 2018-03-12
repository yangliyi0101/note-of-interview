## 反射
反射(Reflection)是Java 程序开发语言的特征之一，它允许运行中的 Java 程序获取自身的信息，并且可以操作类或对象的内部属性。简而言之，通过反射，我们可以在运行时获得程序或程序集中每一个类型的成员和成员的信息。   

反射的核心是JVM在**运行**时可以动态加载类或调用方法/访问属性，它不需要事先（写代码的时候或编译期）知道运行对象是谁。

反射最重要的用途就是开发各种通用框架。很多框架（比如Spring）都是配置化的（比如通过XML文件配置JavaBean,Action之类的），为了保证框架的通用性，它们可能需要根据配置文件加载不同的对象或类，调用不同的方法，这个时候就必须用到反射——**运行时动态加载需要加载的对象**。

例如：spring的bean.xml文件中，定义了一个bean，<bean id="id" class="com.xy.Student" />。 Spring将采用反射的方式创建bean。
	
	Class c = Class.forName("com.xy.Student");
	Object bean = c.newInstance();

## 反射的基本用途
1）获得class对象的方式，主要有三种 1. Class.forName(driver); 通过Class类的forName静态方法。2.调用类的class属性，Person.class返回的就是Person的class对象。3.调用某个对象的getClass()方法。  
2）判断是否为某个类的实例。Class对象的isInstance()方法来判断是否为某个类的实例，它是一个Native方法。  
3）使用Class对象的newInstance()方法来创建Class对象对应类的实例，这种方式采用默认构造方法。而先通过Class对象获取指定的Constructor对象（通过getConstructor（）方法），再调用Constructor对象的newInstance()方法来创建实例。这种方法可以用指定的构造器构造类的实例。  
4）获取方法。getDeclaredMethods()方法返回类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。getMethods()方法返回某个类的所有公用（public）方法，包括其继承类的公用方法。getMethod方法返回一个特定的方法，其中第一个参数为方法名称，后面的参数为方法的参数对应Class的对象。  
5)获取类的成员变量（字段）信息。getFiled: 访问公有的成员变量，可以得到父类的成员变量。getDeclaredField：所有已声明的成员变量。但不能得到其父类的成员变量。  
6）调用方法。当我们从类中获取了一个方法后，我们就可以用invoke()方法来调用这个方法。

	public class test1 {
	    public static void main(String[] args) throws IllegalAccessException, InstantiationException, NoSuchMethodException, 			InvocationTargetException {
	        Class<?> klass = methodClass.class;
	        //创建methodClass的实例
	        Object obj = klass.newInstance();
	        //获取methodClass类的add方法
	        Method method = klass.getMethod("add",int.class,int.class);
	        //调用method对应的方法 => add(1,4)
	        Object result = method.invoke(obj,1,4);
	        System.out.println(result);
	    }
	}
7）利用反射创建数组。其中的Array类为java.lang.reflect.Array类。我们通过Array.newInstance()创建数组对象 

	public static void testArray() throws ClassNotFoundException {
        Class<?> cls = Class.forName("java.lang.String");
        Object array = Array.newInstance(cls,25);
        //往数组里添加内容
        Array.set(array,0,"hello");
        Array.set(array,1,"Java");
        Array.set(array,2,"fuck");
        Array.set(array,3,"Scala");
        Array.set(array,4,"Clojure");
        //获取某一项的内容
        System.out.println(Array.get(array,3));
    }

反射的一些缺限：由于反射会消耗一定的系统资源，因此如果不需要动态地创建一个对象时，就不需要用到反射。另外，反射调用方法时可以忽略权限检查，因此可能会破环封装性而导致安全问题。