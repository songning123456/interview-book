#### 解释下Serialization和Deserialization？
把对象转换为字节序列的过程称为对象的序列化；把字节序列恢复为对象的过程称为对象的反序列化。


对象的序列化主要有两种用途：把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中；在网络上传送对象的字节序列。


当两个进程在进行远程通信时，彼此可以发送各种类型的数据。无论是何种类型的数据，都会以二进制序列的形式在网络上传送。发送方需要把这个Java对象转换为字节序列，才能在网络上传送，接收方则需要把字节序列再恢复为Java对象。


#### Spring的优点?
降低了组件之间的耦合性 ，实现了软件各层之间的解耦。


可以使用容易提供的众多服务，如事务管理，消息服务等。


容器提供单例模式支持。


容器提供了AOP技术，利用它很容易实现如权限拦截，运行期监控等功能。


容器提供了众多的辅助类，能加快应用的开发。


Spring对于主流的应用框架提供了集成支持，如hibernate、JPA、Struts等。


Spring属于低侵入式设计，代码的污染极低。


独立于各种应用服务器。


Spring的DI机制降低了业务对象替换的复杂性。


Spring的高度开放性，并不强制应用完全依赖于Spring，开发者可以自由选择Spring的部分或全部。


#### SpringBean的加载过程？
![SpringBean](/images/SSM/SpringBean.jpeg)


#### Spring中bean实例化的方式(依赖注入)?
Set方法
```
public class SpringAction {  
    private SpringDao springDao;  
    public void setSpringDao(SpringDao springDao) {  
        this.springDao = springDao;
    }  
    public void doMethod(){  
        springDao.doMethod();  
    }  
}  

<bean name="springAction" class="xxx.SpringAction">  
    <property name="springDao" ref="springDao"></property>
</bean>  
<bean name="springDao" class="xxx.impl.SpringDaoImpl"></bean>  
```

普通构造方法
```
public class SpringAction {  
    private SpringDao springDao;   
    public SpringAction(SpringDao springDao){  
        this.springDao = springDao;  
    }
    public void doMethod(){  
        springDao.doMethod();  
    }  
}  

 <bean name="springAction" class="xxx.SpringAction">  
    <constructor-arg index="0" ref="springDao"></constructor-arg>  
</bean>
<bean name="springDao" class="xxx.impl.SpringDaoImpl"></bean>  
```


静态工厂
```
public class SpringAction {  
    private FactoryDao staticFactoryDao;   
    public void setStaticFactoryDao(FactoryDao staticFactoryDao) {  
        this.staticFactoryDao = staticFactoryDao;  
    }  
}  

public class DaoFactory {  
    public static final FactoryDao getStaticFactoryDaoImpl(){  
        return new StaticFacotryDaoImpl();  
    }  
} 

<bean id="bean2" class="xxx.StaticFactory" factory-method="getInstance"/>
<bean name="springAction" class="xxx.SpringAction" >  
    <property name="staticFactoryDao" ref="staticFactoryDao"></property>
</bean>  
<bean name="staticFactoryDao" class="xxx.DaoFactory" factory-method="getStaticFactoryDaoImpl"></bean>  
```


实例工厂创建
```
public class SpringAction {  
    private FactoryDao factoryDao;  
    public void setFactoryDao(FactoryDao factoryDao) {  
        this.factoryDao = factoryDao;  
    }  
}  

public class DaoFactory {   
    public FactoryDao getFactoryDaoImpl(){  
        return new FactoryDaoImpl();  
    }  
}  

<bean name="springAction" class="xxx.SpringAction">   
    <property name="factoryDao" ref="factoryDao"></property>  
</bean>  
<bean name="daoFactory" class="xxx.DaoFactory"></bean>  
<bean name="factoryDao" factory-bean="daoFactory" factory-method="getFactoryDaoImpl"></bean>  
```


#### Spring IOC原理?
IOC(Inversion Of Control)是指容器控制程序对象之间的关系，而不是传统实现中，由程序代码直接操控。控制权由应用代码中转到了外部容器，控制权的转移是所谓反转。对于Spring而言，就是由Spring来控制对象的生命周期和对象之间的关系；IOC还有另外一个名字——“依赖注入(Dependency Injection)”。从名字上理解，所谓依赖注入，即组件之间的依赖关系由容器在运行期决定，即由容器动态地将某种依赖关系注入到组件之中。


所有的类都会在Spring容器中登记，告诉Spring这是个什么东西，你需要什么东西，然后Spring会在系统运行到适当的时候，把你要的东西主动给你，同时也把你交给其他需要你的东西。所有的类的创建、销毁都由Spring来控制，也就是说控制对象生存周期的不再是引用它的对象，而是Spring。对于某个具体的对象而言，以前是它控制其他对象，现在是所有对象都被Spring控制，所以这叫控制反转。


在系统运行中，动态的向某个对象提供它所需要的其他对象。


依赖注入的思想是通过反射机制实现的，在实例化一个类时，它通过反射调用类中set方法将事先保存在HashMap中的类属性注入到类中。


总而言之，在传统的对象创建方式中，通常由调用者来创建被调用者的实例，而在Spring中创建被调用者的工作由Spring来完成，然后注入调用者，即所谓的依赖注入or控制反转。


注入方式有两种：依赖注入和设置注入。


IOC的优点：降低了组件之间的耦合，降低了业务对象之间替换的复杂性，使之能够灵活的管理对象。


#### Spring AOP原理?
AOP面向方面编程基于IOC，是对OOP的有益补充。


AOP利用一种称为“横切”的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其名为“Aspect”，即方面。所谓“方面”，简单地说，就是将那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，比如日志记录，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可操作性和可维护性。


AOP代表的是一个横向的关系，将“对象”比作一个空心的圆柱体，其中封装的是对象的属性和行为；则面向方面编程的方法，就是将这个圆柱体以切面形式剖开，选择性的提供业务逻辑。而剖开的切面，也就是所谓的“方面”了。然后它又以巧夺天功的妙手将这些剖开的切面复原，不留痕迹，但完成了效果。


实现AOP的技术，主要分为两大类：一是采用动态代理技术，利用截取消息的方式，对该消息进行装饰，以取代原有对象行为的执行；二是采用静态织入的方式，引入特定的语法创建“方面”，从而使得编译器可以在编译期间织入有关“方面”的代码。


| 代理方式 | 解释 | 
| :----- | :----- | 
| <div style="width: 100px">JDK动态代理</div> | 其代理对象必须是某个接口的实现，它是通过在运行期间创建一个接口的实现类来完成对目标对象的代理，其核心的两个类是InvocationHandler和Proxy | 
| <div style="width: 100px">CGLIB代理</div> | 实现原理类似于JDK动态代理，只是它在运行期间生成的代理对象是针对目标类扩展的子类。CGLIB是高效的代码生成包，底层是依靠ASM(开源的java字节码编辑类库)操作字节码实现的，性能比JDK强；需要引入包asm.jar和cglib.jar | 


#### Spring事务的传播行为？
[测试用例](https://blog.csdn.net/qq_26323323/article/details/81908955)


| 传播行为 | 解释 | 
| :----- | :----- | 
| <div style="width: 280px">PROPAGATION_REQUIRED</div> | 表示当前方法必须运行在事务中。如果当前事务存在，方法将会在该事务中运行。否则，会启动一个新的事务 | 
| <div style="width: 280px">PROPAGATION_SUPPORTS</div> | 表示当前方法不需要事务上下文，但是如果存在当前事务的话，那么该方法会在这个事务中运行 | 
| <div style="width: 280px">PROPAGATION_MANDATORY</div> | 表示该方法必须在事务中运行，如果当前事务不存在，则会抛出一个异常 | 
| <div style="width: 280px">PROPAGATION_REQUIRED_NEW</div> | 表示当前方法必须运行在它自己的事务中。一个新的事务将被启动。如果存在当前事务，在该方法执行期间，当前事务会被挂起。如果使用JTATransactionManager的话，则需要访问TransactionManager | 
| <div style="width: 280px">PROPAGATION_NOT_SUPPORTED</div> | 表示该方法不应该运行在事务中。如果存在当前事务，在该方法运行期间，当前事务将被挂起。如果使用JTATransactionManager的话，则需要访问TransactionManager | 
| <div style="width: 280px">PROPAGATION_NEVER</div> | 表示当前方法不应该运行在事务上下文中。如果当前正有一个事务在运行，则会抛出异常 | 
| <div style="width: 280px">PROPAGATION_NESTED</div> | 表示如果当前已经存在一个事务，那么该方法将会在嵌套事务中运行。嵌套的事务可以独立于当前事务进行单独地提交或回滚。如果当前事务不存在，那么其行为与PROPAGATION_REQUIRED一样。注意各厂商对这种传播行为的支持是有所差异的。可以参考资源管理器的文档来确认它们是否支持嵌套事务 |


#### SpringMVC工作原理？
![SpringMVC](/images/SSM/SpringMVC.jpeg)


**步骤1** 客户端发出一个http请求给web服务器，web服务器对http请求进行解析，如果匹配DispatcherServlet的请求映射路径(在web.xml中指定)，web容器将请求转交给DispatcherServlet。


**步骤2** DispatcherServlet接收到这个请求之后将根据请求的信息(包括URL、Http方法、请求报文头和请求参数Cookie等)以及HandlerMapping的配置找到处理请求的处理器(Handler)。


**步骤3&&4** DispatcherServlet根据HandlerMapping找到对应的Handler,将处理权交给Handler(Handler将具体的处理进行封装)，再由具体的HandlerAdapter对Handler进行具体的调用。


**步骤5** Handler对数据处理完成以后将返回一个ModelAndView()对象给DispatcherServlet。


**步骤6** Handler返回的ModelAndView()只是一个逻辑视图并不是一个正式的视图，DispatcherServlet通过ViewResolver将逻辑视图转化为真正的视图View。


**步骤7** DispatcherServlet通过model解析出ModelAndView()中的参数进行解析最终展现出完整的view并返回给客户端。


#### SpringMVC常用的注解？
| 名称 | 解释 | 
| :----- | :----- | 
| @Controller | Controller控制器是通过服务接口定义的提供访问应用程序的一种行为，它解释用户的输入，将其转换成一个模型然后将试图呈献给用户。Spring MVC使用@Controller定义控制器，它还允许自动检测定义在类路径下的组件并自动注册。如想自动检测生效，需在XML头文件下引入spring-context | 
| @RequestMapping | 既可以作用在类级别，也可以作用在方法级别 | 
| @PathVariable | @PathVariable中的参数可以是任意的简单类型，如int, long, Date等等。Spring会自动将其转换成合适的类型或者抛出TypeMismatchException异常 | 
| @RequestParam | @RequestParam将请求的参数绑定到方法中的参数上。其实，即使不配置该参数，注解也会默认使用该参数。如果想自定义指定参数的话，如果将@RequestParam的required属性设置为false(如@RequestParam(value="id",required=false)) | 
| @RequestBody | 方法参数应该被绑定到HTTP请求Body上 | 
| @ResponseBody | 将返回类型直接输入到HTTP response body中 | 
| @RestController | 创建REST类型的控制器与@Controller类型 | 


#### Restful的优势？
| 优势 | 解释 | 
| :----- | :----- | 
| <div style="width: 150px">客户-服务器</div> | 客户-服务器约束背后的原则是分离关注点。通过分离用户接口和数据存储这两个关注点，改善了用户接口跨多个平台的可移植性；同时通过简化服务器组件，改善了系统的可伸缩性 | 
| <div style="width: 150px">无状态</div> | 通信在本质上是无状态的，改善了可见性、可靠性、可伸缩性 | 
| <div style="width: 150px">缓存</div> | 改善了网络效率减少一系列交互的平均延迟时间，来提高效率、可伸缩性和用户可觉察的性能 | 
| <div style="width: 150px">统一接口</div> | REST架构风格区别于其他基于网络的架构风格的核心特征是，它强调组件之间要有一个统一的接口 | 


#### 什么是JDBC？
JDBC是允许用户在不同数据库之间做选择的一个抽象层。JDBC允许开发者用JAVA写数据库应用程序，而不需要关心底层特定数据库的细节。


#### 解释下驱动(Driver)在JDBC中的角色？
JDBC驱动提供了特定厂商对JDBC API接口类的实现，驱动必须要提供java.sql包下面这些类的实现：Connection, Statement, PreparedStatement,CallableStatement, ResultSet和Driver。


#### Class.forName()方法有什么作用？
这个方法用来载入跟数据库建立连接的驱动。


#### PreparedStatement比Statement有什么优势？
| Statement | PreparedStatement | 
| :----- | :----- | 
| 每次执行sql语句，相关数据库都要执行sql语句的编译 | PreparedStatement是预编译的，对于批量处理可以大大提高效率，也叫JDBC存储过程 | 
| 在对数据库只执行一次性存取的时侯，用Statement 对象进行处理 | 对象的开销比Statement大，对于一次性操作并不会带来额外的好处 | 


#### 什么时候使用CallableStatement？用来准备CallableStatement的方法是什么？
CallableStatement用来执行存储过程。存储过程是由数据库存储和提供的。存储过程可以接受输入参数，也可以有返回结果。非常鼓励使用存储过程，因为它提供了安全性和模块化。准备一个CallableStatement的方法是：CallableStament.prepareCall()。


#### 数据库连接池是什么意思？
像打开关闭数据库连接这种和数据库的交互可能是很费时的，尤其是当客户端数量增加的时候，会消耗大量的资源，成本是非常高的。可以在应用服务器启动的时候建立很多个数据库连接并维护在一个池中。连接请求由池中的连接提供。在连接使用完毕以后，把连接归还到池中，以用于满足将来更多的请求。

