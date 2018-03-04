## 传统的JDBC编程 和 ORM模型
传统的JDBC编程大致分为如下几个步骤：  
1. 使用JDBC编程需要连接数据库，注册驱动和数据库信息。  
2. 操作Connection，打开Statement对象。  
3. 通过Statement执行SQL，返回结果到ResultSet对象。  
4. 使用ResultSet读取数据，然后通过代码转化为具体的POJO对象。  
5. 关闭数据库相关资源。  

使用传统的JDBC方式存在一些弊端。其一，工作量相对较大。我们需要先连接，然后处理JDBC底层事务。我们还需要操作Connection对象、Statement对象和ResultSet对象去拿到数据，并准确关闭它们。其二，我们要对JDBC编程可能产生的异常进行捕捉处理并正确关闭。 很快这种方式被ORM模型取代。  
ORM模型就是数据库的表和简单Java对象的映射关系模型。有了ORM模型，在大部分情况下，程序员只需要了解Java应用而无需对数据库相关知识深入了解，便可以写出通俗易懂的程序。

## Hibernate MyBatis
Hibernate是建立在若干POJO通过XML映射文件提供的规则映射到数据库表上的，是一个非常优秀的ORM模型。在配置了映射文件和数据库连接文件之后，Hibernate就可以通过Session操作，非常容易，消除了JDBC带来的大量代码，大大提高了编程的简易性和可读性。此外，它还提供级联、缓存、映射、一对多等功能。Hibernate是全表映射，你可以通过HQL去操作POJO进而操作数据库的数据。  
Hibernate的缺点：  
1. 全表映射带来的不便，比如更新时需要发送所有的字段。  
2. 无法根据不同的条件组装不同的SQL。  
3. 对多表关联和复杂SQL查询支持较差，需要自己写SQL，返回后，需要自己将数据组装为POJO。  
4. 不能有效支持存储过程。  
5. 虽然有HQL，但是性能较差，大型互联网系统往往需要优化SQL，而Hibernate做不到。   
 
一个半自动化映射的框架MyBatis应运而生。与Hibernate不同的是，它不单单要我们提供映射文件，还需要我们提供SQL语句。MyBatis所需要提供的映射文件包含以下三个部分：SQL、映射规则、POJO。当你需要一个灵活的、可以动态生成映射关系的框架，那么MyBatis确实是一个最好的选择。它几乎可以替代JDBC，拥有动态列、动态表名、存储过程都支持，同时提供了简易的缓存、日志、级联。但是它的缺陷是需要你提供映射规则和SQL，所以它的开发工作量比Hibernate略大一些。Mybatis和hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句，不过mybatis可以通过XML或注解方式灵活配置要运行的sql语句，并将java对象和sql语句映射生成最终执行的sql，最后将sql执行的结果再映射生成java对象。  

##MyBatis编程步骤
1. 创建SqlSessionFactory
2. 通过SqlSessionFactory创建SqlSession
3. 通过sqlsession执行数据库操作
4. 调用session.commit()提交事务
5. 调用session.close()关闭会话

## 简单的说一下MyBatis的一级缓存和二级缓存
Mybatis对缓存提供支持，默认只开启一级缓存（一级缓存只是相对于同一个SqlSession而言）。Mybatis首先去缓存中查询结果集，如果没有则查询数据库，如果有则从缓存取出返回结果集就不走数据库。Mybatis内部存储缓存使用一个HashMap，key为hashCode+sqlId+Sql语句。value为从查询出来映射生成的java对象。在参数和SQL完全一样的情况下，我们使用同一个SqlSession对象调用同一个Mapper的方法，先去查缓存里的。  
而二级缓存是在SqlSessionFactory层面所共享的，可以跨SqlSession的。

## MyBatis 动态 sql 是什么？
mybatis 在 xml 映射文件中提供了一套动态 sql 拼接的逻辑标签，通过这些标签我们可以根据实际传入参数的情况进行 sql 的拼接，最终达到根据参数动态生成 sql 的效果。

## Mapper 接口的工作原理
Mapper仅仅是一个接口，而不是一个包含逻辑的实体类，我们知道一个接口是没有办法去执行的，很显然Mapper产生了代理类，这个代理类是由MyBatis为我们创建的。
mybatis 在运行时使用 JDK 动态代理生成代理对象，在调用接口方法时，代理对象会拦截接口方法，转而运行 MappedStatement 的 sql 语句，并生成结果返回。JDK动态代理的最大缺点就是需要提供接口，而MyBatis的Mapper就是一个接口，它采用的就是JDK的动态代理。  

## ${} 和 #{} 的区别  

   ${} 时 property 文件的变量占位符，可以运用在标签属性值和 sql 内部，属于静态文本替换，${} 中的内容会直接被替换成变量对应的值。  
   而#{} 属于参数占位符，mybatis 会在预编译过程中把对应的参数替换成 sql 中？进行填充，并且在执行 sql 前通过反射机制获取对应参数对象的值，并按照参数占位符进行参数的设置，这样参数占位符可以防止 sql 注入。
