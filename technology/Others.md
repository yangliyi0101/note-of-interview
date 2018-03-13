## WebSocket
已经有HTTP了，为什么还需要WebSocket？因为 HTTP 协议有一个缺陷：通信只能由客户端发起。  

WebSocket的最大特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。
其他特点：  
（1）建立在 TCP 协议之上，服务器端的实现比较容易。  
（2）与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。  
（3）数据格式比较轻量，性能开销小，通信高效。  
（4）可以发送文本，也可以发送二进制数据。  
（5）没有同源限制，客户端可以与任意服务器通信。  
（6）协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。
	
	ws://example.com:80/some/path
	
http://www.ruanyifeng.com/blog/2017/05/websocket.html

## WebService
Web service让你的网站可以使用其他网站的资源，比如在网页上显示天气、地图、twitter上的最新动态等等。   
Web Service架构的基本思想，就是尽量把非核心功能交给其他人去做，自己全力开发核心功能。    

## IaaS，PaaS，SaaS的区别
你可以从头到尾，自己生产披萨，但是这样比较麻烦，需要准备的东西多，因此你决定外包一部分工作，采用他人的服务。你有三个方案。

IaaS：基础设施服务，Infrastructure-as-a-service 他人提供厨房、炉子、煤气，你使用这些基础设施，来烤你的披萨。 

PaaS：平台服务，Platform-as-a-service 除了基础设施，他人还提供披萨饼皮。

SaaS：软件服务，Software-as-a-service 他人直接做好了披萨，不用你的介入，到手的就是一个成品。你要做的就是把它卖出去，最多再包装一下，印上你自己的 Logo。

## Maven依赖的jar包版本冲突解决
Maven是目前管理jar包最流行的工具。然而过多的jar包依赖可能会导致版本冲突的问题。
	
	项目依赖A和B；A依赖C1.0，B依赖C2.0，如果C的两个版本不兼容，我们的项目就会出现问题。
像例子中这样的C在还是很多的，最常见的就属apache的一堆工具包，比如commons-logging;   
**冲突解决方法：**  
1、分析冲突的jar包的依赖路径：  

	mvn dependency:tree -Dverbose -Dincludes=commons-logging:commons-loggging
这条命令可以打印出所有依赖了groupId和artifactId都为commons-logging的jar包的依赖路径。（groupId和artifactId合在一起统称为坐标）  
2、选择一个所需的版本。在两个冲突的版本中，我们在自己的项目中用到了哪个版本的语法，就选择哪个。   
3 在本项目的pom中将冲突的依赖排除：

	<dependency>
		<groupId>com.xxx.xx</groupId>
		<artifactId>xxx</artifactId>
		<version>x</version>
		<exclusions>
			<exclusion>
			<artifactId>com.springsource.slf4j.org.apache.commons.logging</artifactId>
			<groupId>org.slf4j</groupId>
			</exclusion>
		</exclusions>
	</dependency>
小tips：最后推荐一下intellij，在intellij中打开你的pom.xml,右键单击内容，选择diagram->show dependencies, 会自动执行mvn:dependency 并绘制一个依赖树，列出所有的依赖包如下图所示。想要排除哪个，可以直接右键选择"exclude".不过目前我还没发现怎样像命令行一样过滤树的结果，所有如果依赖太多，还是先执行一下命令找出哪里的依赖冲突了比较好。
