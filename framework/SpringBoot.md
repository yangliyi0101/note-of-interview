## SpringBoot
Spring Boot是Spring社区发布的一个开源项目，旨在帮助开发者快速并且更简单的构建项目。大多数SpringBoot项目只需要很少的配置文件。

核心功能：

1. 独立运行Spring项目 

	SpringBoot可以以jar包形式独立运行，运行一个SpringBoot项目只需要 java -jar xx.jar来运行。
2. 内置servlet容器

	Spring Boot可以选择内嵌Tomcat、jetty或者Undertow,这样我们无须以war包形式部署项目。
3. 提供starter简化Maven配置

	Spring提供了一系列的starter pom来简化Maven的依赖加载，例如，当你使用了spring-boot-starter-web，会自动加入很多web相关的依赖包。spring-boot-starter-parent：是一个特殊Start，它用来提供相关的Maven依赖项，使用它之后，常用的包依赖可以省去version标签。
4. 自动装配Spring

	SpringBoot会根据在类路径中的jar包，类、为jar包里面的类自动配置Bean，这样会极大地减少我们要使用的配置。当然，SpringBoot只考虑大多数的开发场景，并不是所有的场景，若在实际开发中我们需要配置Bean，而SpringBoot也有提供支持，则可以自定义自动配置。
5. 准生产的应用监控

	SpringBoot提供基于http ssh telnet对运行时的项目进行监控。
6. 无代码生产和xml配置

	SpringBoot不是借助xml与代码生成来实现的，而是通过条件注解来实现的，这是Spring4.x提供的新特性。

特性：

创建独立的Spring项目  
内置Tomcat和Jetty容器  
提供一个starter POMs来简化Maven配置  
提供了一系列大型项目中常见的非功能性特性，如安全、指标，健康检测、外部配置等  
完全没有代码生成和xml配置文件   

## Spring Cloud
SpringCloud 是Pivotal提供的用于**简化分布式系统构建的工具集**。Spring Cloud引入了云平台连接器(CloudConnector)和服务连接器(Service Connector)的概念。云平台连接器是一个接口，需要由云平台提供者进行实现，以便库中的其他模块可以与该云平台协同工作。   
Spring Cloud从技术架构上降低了对大型系统构建的要求，使我们以非常低的成本（技术或者硬件）搭建一套高效、分布式、容错的平台，但Spring Cloud也不是没有缺点，小型独立的项目不适合使用。    
Spring Cloud必须依赖于Spring Boot
**轻轻松松几行代码就完成了熔断、负载均衡、服务中心的各种平台功能**
https://zhuanlan.zhihu.com/p/27435507   

熔断机制   
Hystrix熔断机制是为系统调用设置一定的失败数量，分为三种状态：Open、Closed、Half-Open。当请求超过设置的失败数量时，熔断机制会将状态切换到Open，这时所有请求都会失败，不会再发送请求到服务端。当Open状态持续一定时间，会自动切换到Half-Open状态，此时当一个请求继续失败，熔断器又会切回Open状态，若此时请求成功，熔断器会切回Closed状态。Hystrix的熔断机制就像家庭电路中的保险丝，发现微服务不可用，熔断器会自动切断请求，避免发送大量无效请求影响系统吞吐量，熔断机制提供强大的检测以及恢复的能力。   
http://www.cnblogs.com/myfrank/p/7511233.html