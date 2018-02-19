## 什么是SpringMVC
SpringMVC是Spring框架的一个模块，SpringMVC和Spring无需通过中间整合层进行开发。  
SpringMVC是一个基于MVC的web框架。  
MVC是一种设计模式。M代表model；V代表view；C代表controller。M指的是模型，即dao层和service层，V指的是视图，即jsp html 等。C指的是controller，即控制器。

## SpringMVC工作原理
![](https://images0.cnblogs.com/blog2015/712052/201507/191439034705767.png)  
(1).发起请求到前端控制器(DispatcherServlet);  
(2).前端控制器请求HandlerMapping查找Handler，可以根据xml配置、注解进行查找；  
(3).处理器映射器HandlerMapping将请求映射为HandleExecutionChain对象（包含Handle对象，多个HandlerInterceptor对象）。并返回该处理器链给前端控制器。  
(4).前端控制器调用处理器适配器去执行Handler；  
(5).处理器适配器去执行Handler；  
(6).Handler执行完成给适配器返回ModelAndView；  
(7).处理器适配器向前端控制器返回ModelAndView(是springmvc框架的一个底层对象，包括Model和View)；  
(8).前端控制器请求视图解析器去进行视图解析，根据逻辑视图名称解析真正的视图(jsp...)；  
(9).视图解析器向前端控制器返回View;  
(10).前端控制器进行视图渲染，视图渲染就是将模型数据(在ModelAndView对象中)填充到request域中。  
(11).前端控制器向用户响应结果。  
**组件：**  
1、**前端控制器DispatcherServlet**（不需要攻城狮开发）,由框架提供。  
作用：接收请求，响应结果，相当于转发器，中央处理器。  
有了dispatcherServlet减少了其它组件之间的耦合度。用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。  
2、**处理器映射器HandlerMapping**(不需要攻城狮开发),由框架提供  
作用：根据请求的url查找Handler  
HandlerMapping负责根据用户请求找到Handler即处理器，springmvc提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。
3、**处理器适配器HandlerAdapter**  
作用：按照特定规则（HandlerAdapter要求的规则）去执行Handler  
通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。  
4、**处理器Handler**(需要攻城狮开发)  
注意：编写Handler时按照HandlerAdapter的要求去做，这样适配器才可以去正确执行Handler  
Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。  
5、**视图解析器View resolver**(不需要攻城狮开发),由框架提供  
作用：进行视图解析，根据逻辑视图名解析成真正的视图（view）  
View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。 springmvc框架提供了很多的View视图类型，包括：jstlView、freemarkerView、pdfView等。  

## SpringMVC的优点
1.Spring MVC 中提供了一个Dispatcher Servlet，无需额外开发。   
2.Spring MVC 中使用基于XML的配置文件，可以编辑，而无需重新编译应用程序。  
3.Spring MVC 实例化控制器，并根据用户输入来构造bean。  
4.Spring MVC 可以自动绑定用户输入，并正确地转换数据类型。例如，Spring MVC能自动解析字符串，并设置float或decimal类型的属性。  
5.Spring MVC可以校验用户输入，若校验不通过，则重定向回输入表单。输入校验是可选的，支持编程方式以及声明。关于这一点，Spring MVC内置了常见的校验器。  
6.Spring MVC是Spring框架的一部分。可以利用Spring提供的其他功能（例如依赖注入功能，@Autowired)。  
7.Spring MVC支持国际化和本地化。支持根据用户区域显示多国语言。  
8.Spring MVC支持多种视图技术。最常见的JSP技术以及其他技术包括Velocity和FreeMarker。  

## 基于注释的控制器 
Spring2.5之前，老的项目通过Spring MVC配置文件来生成控制器，映射控制器和url的关系，例如：  

	<bean name="/product_input.action" class="app03a.controller.InputProductController"/> 
	
而Controller层代码里，InputProductController实现了Controller接口。
Spring 2.5版本引入了一个新的途径：通过使用控制器来注释类型。基于注解的控制器有两个优点：1、一个控制器类可以处理多个动作（而一个实现了Controller接口的控制器只能处理一个动作）。这就允许将相关的操作写在同一个控制器类，从而减少应用程序中类的数量。2、基于注解的控制器的请求映射不需要存储在配置文件中。使用RequestMapping注释类型，可以对一个方法进行请求处理。  
@Controller 用于指示Spring类的实例是一个控制器。  
Spring使用扫描机制来找到应用程序中所有基于注解的控制器类。<context:component-scan base-package=""/>  
@RequestMapping 用来映射一个请求和一个方法。

## @Autowired和@Resource的区别
相同点：@Autowired与@Resource都可以用来装配bean. 都可以写在字段上,或写在setter方法上。   
**@Autowired**默认按**类型**装配（这个注解是属于spring的），默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用:

	@Autowired() @Qualifier("baseDao")    
	private BaseDao baseDao;
**@Resource** 是JDK1.6支持的注解，默认按照名称进行装配，名称可以通过name属性进行指定，如果没有指定name属性，当注解写在字段上时，默认取字段名，按照名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。

## @RequestParam @RequestBody @PathVariable 等参数绑定注解详解
**@PathVariable**：当使用@RequestMapping URI template 样式映射时， 即 someUrl/{paramId}, 这时的paramId可通过 @Pathvariable注解绑定它传过来的值到方法的参数上。  
**@RequestHeader** 注解，可以把Request请求header部分的值绑定到方法的参数上。  
**@CookieValue** 可以把Request header中关于cookie的值绑定到方法的参数上。  
**@RequestParam** 常用来处理简单类型的绑定，通过Request.getParameter() 获取的String可直接转换为简单类型的情况  和 用来处理Content-Type: 为 application/x-www-form-urlencoded编码的内容，提交方式GET、POST；  
**@RequestBody** 该注解常用来处理Content-Type: 不是application/x-www-form-urlencoded编码的内容，例如application/json, application/xml等；它是通过使用HandlerAdapter 配置的HttpMessageConverters来解析post data body，然后绑定到相应的bean上的。    
**@SessionAttributes** 该注解用来绑定HttpSession中的attribute对象的值，便于在方法中的参数里使用。  
**@ModelAttribute** 用于方法上时：  通常用来在处理@RequestMapping之前，为请求绑定需要从后台查询的model；  
用于方法上时：  通常用来在处理@RequestMapping之前，为请求绑定需要从后台查询的model；  

	@ModelAttribute  
	public Account addAccount(@RequestParam String number) {  
	    return accountManager.findAccount(number);  
	} 
这种方式实际的效果就是在调用@RequestMapping的方法之前，为request对象的model里put（“account”， Account）；  
用于参数上时： 用来通过名称对应，把相应名称的值绑定到注解的参数bean上；要绑定的值来源于：  
A） @SessionAttributes 启用的attribute 对象上；  
B） @ModelAttribute 用于方法上时指定的model对象；  
C） 上述两种情况都没有时，new一个需要绑定的bean对象，然后把request中按名称对应的方式把值绑定到bean中。 
 
	@RequestMapping(value="/owners/{ownerId}/pets/{petId}/edit", method = RequestMethod.POST)  
	public String processSubmit(@ModelAttribute Pet pet) {  
     
	} 	
首先查询 @SessionAttributes有无绑定的Pet对象，若没有则查询@ModelAttribute方法层面上是否绑定了Pet对象，若没有则将URI template中的值按对应的名称绑定到Pet对象的各属性上。

http://blog.csdn.net/walkerjong/article/details/7946109

## SpringMVC和Struts2区别
**拦截级别** Struts2是**类级别**的拦截， 一个类对应一个request上下文，SpringMVC是**方法级别**的拦截，一个方法对应一个request上下文，而方法同时又跟一个url对应,所以说从架构本身上SpringMVC就容易实现restful.而struts2的架构实现起来要费劲，虽然Struts2中Action的一个方法可以对应一个url，而其类属性却被所有方法共享，这也就无法用注解或其他方式标识其所属方法了。  
**数据独立性** SpringMVC的方法之间基本上独立的，独享request response数据，请求数据通过参数获取，处理结果通过ModelMap交回给框架，**方法之间不共享变量**，而Struts2搞的就比较乱，虽然方法之间也是独立的，但其一个Action下的变量是共享的，这不会影响程序运行，却给我们编码 读程序时带来麻烦，每次来了请求就创建一个Action，一个Action对象对应一个request上下文。  
**内存损耗** 由于Struts2需要针对每个request进行封装，把request，session等servlet生命周期的变量封装成一个一个Map，供给每个Action使用，并保证线程安全，所以在原则上，是比较耗费内存的。   
**拦截器机制** 拦截器实现机制上，Struts2有以自己的interceptor机制，SpringMVC用的是独立的AOP方式，这样导致Struts2的配置文件量还是比SpringMVC大。  
**入口的不同** SpringMVC的入口是servlet，而Struts2是filter（这里要指出，filter和servlet是不同的。以前认为filter是servlet的一种特殊实现），这就导致了二者的机制不同，这里就牵涉到servlet和filter的区别了。   
**对Ajax的支持**  SpringMVC集成了Ajax，使用非常方便，只需一个注解@ResponseBody就可以实现，然后直接返回响应文本即可(只支持异步调用)，而Struts2拦截器集成了Ajax，在Action中处理时一般必须安装插件或者自己写代码集成进去，使用起来也相对不方便   
**验证机制**  SpringMVC验证支持JSR303，处理起来相对更加灵活方便，而Struts2验证比较繁琐，感觉太烦乱。   
**和Spring关系**  spring MVC和Spring是无缝的。从这个项目的管理和安全上也比Struts2高（当然Struts2也可以通过不同的目录结构和相关配置做到SpringMVC一样的效果，但是需要xml配置的地方不少）。  




