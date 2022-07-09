## 题卷VI


![](/images/ReviewVI/ReviewVI.png)


### Java基础


#### JDK和JRE有什么区别？
|名称|解释|
| :----- | :----- |
|JDK|Java Development Kit的简称，java开发工具包，提供了java的开发环境和运行环境。|
|JRE|Java Runtime Environment的简称，java运行环境，为java的运行提供了所需环境。|


具体来说JDK其实包含了JRE，同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析的工具。简单来说如果你需要运行java程序，只需安装JRE就可以了，如果你需要编写java程序，需要安装JDK。


#### ==和equals的区别是什么？
**==**


1. 基本类型: 比较的是值是否相同；
2. 引用类型: 比较的是引用是否相同。


```java
String x = "string";
String y = "string";
String z = new String("string");
System.out.println(x==y); // true
System.out.println(x==z); // false
System.out.println(x.equals(y)); // true
System.out.println(x.equals(z)); // true
```


因为x和y指向的是同一个引用，所以==也是true，而new String()方法则重写开辟了内存空间，所以==结果为 false，而equals比较的一直是值，所以结果都为true。


**equals**


equals本质上就是==，只不过String和Integer等重写了equals方法，把它变成了值比较。


```java
class Cat {

    private String name;
    
    public Cat(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

Cat c1 = new Cat("王磊");
Cat c2 = new Cat("王磊");
System.out.println(c1.equals(c2)); // false
```


```java
// equals本质上就是==
public boolean equals(Object obj) {
		return (this == obj);
}
```


**两个相同值的String对象，为什么返回的是true？**


```java
String s1 = new String("老王");
String s2 = new String("老王");
System.out.println(s1.equals(s2)); // true
```


```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```


String重写了Object的equals方法，把引用比较改成了值比较。


**总结**


==对于基本类型来说是值比较，对于引用类型来说是比较的是引用；而equals默认情况下是引用比较，只是很多类重写了equals方法，比如String、Integer等把它变成了值比较，所以一般情况下equals比较的是值是否相等。


#### 两个对象的hashCode()相同，则equals()也一定为true？


两个对象的hashCode()相同，equals()不一定true。


```java
String str1 = "通话";
String str2 = "重地";
System.out.println(String.format("str1: %d | str2: %d", str1.hashCode(), str2.hashCode()));
System.out.println(str1.equals(str2));
```


```java
str1: 1179395 | str2: 1179395

false
```


很显然“通话”和“重地”的hashCode()相同，然而equals()则为false，因为在散列表中，hashCode()相等即两个键值对的哈希值相等，然而哈希值相等并不一定能得出键值对相等。


#### final在Java中有什么作用？
|使用范围|解释|
| :----- | :----- |
|<div style='width: 80px'>修饰类</div>|被final修饰的类不允许被继承，表示此类设计的很完美，不需要被修改和扩展。|
|<div style='width: 80px'>修饰方法</div>|被final修饰的方法表示此方法提供的功能已经满足当前要求，不需要进行扩展，并且也不允许任何从此类继承的类来重写此方法。|
|<div style='width: 80px'>修饰变量</div>|当final修饰变量时，表示该属性一旦被初始化便不可以被修改。|
|<div style='width: 80px'>修饰参数</div>|当final修饰参数时，表示此参数在整个方法内不允许被修改。|


使用final修饰类可以防止被其他类继承，如JD代码中String类就是被final修饰的，从而防止被其他类继承，导致内部逻辑被破坏。


final是Java中常见的一个关键字，被它修饰的对象不允许修改、替换其原始值或定义。final有4种用法，可以用来修饰类、方法、变量或参数。


#### Math.round(-1.5)等于多少？
-1


Math.round()表示“四舍六入”，将原来的数字加上0.5后再向下取整。


```java
Math.round(x) => Math.floor(x + 0.5)
```


#### String属于基础的数据类型吗？
不属于，String属于对象。


| 基础数据类型 | 字节长度(byte) |
| :----: | :----: |
| byte | 1 |
| short | 2 |
| int | 4 |
| long | 8 |
| boolean | 1 |
| char | 2 |
| float | 4 |
| double | 8 |


#### Java中操作字符串都有哪些类？它们之间有什么区别？
| ———— | 可变性 | 线程安全性 | 性能 | 
| :----- | :----- | :----- | :----- | 
| String | 用字符数组保存字符串，private final char value[]，所以String对象是不可变的。 | 对象是不可变的，也就可以理解为常量，线程安全。 | 每次对String类型进行改变的时候，都会生成一个新的String对象，然后将指针指向新的String对象。 | 
| StringBuffer | 继承自AbstractStringBuilder类，在AbstractStringBuilder中也是使用字符数组保存字符串，char[] value，是可变的。 | 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。 | 本身进行操作，而不是生成新的对象并改变对象引用。 | 
| StringBuilder | 同StringBuffer。 | 并没有对方法进行加同步锁，所以是非线程安全的。 | StringBuilder相比使用StringBuffer仅能获得10%~15%左右的性能提升。 | 


#### String str="i"与String str=new String("i")一样吗？
不一样，因为内存的分配方式不一样。String str="i"的方式，Java虚拟机会将其分配到常量池中；而String str=new String("i")则会被分到堆内存中。


#### 如何将字符串反转？
使用StringBuilder或者StringBuffer的reverse()方法。


```java
// StringBuffer reverse
StringBuffer stringBuffer = new StringBuffer();
stringBuffer.append("abcdefg");
System.out.println(stringBuffer.reverse()); // gfedcba
// StringBuilder reverse
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append("abcdefg");
System.out.println(stringBuilder.reverse()); // gfedcba
```


#### String类的常用方法都有那些？
| 方法名称 | 解释 |
| :----: | :----: |
|indexOf()|返回指定字符的索引。|
|charAt()|返回指定索引处的字符。|
|replace()|字符串替换。|
|trim()|去除字符串两端空白。|
|split()|分割字符串，返回一个分割后的字符串数组。|
|getBytes()|返回字符串的byte类型数组。|
|length()|返回字符串长度。|
|toLowerCase()|将字符串转成小写字母。|
|toUpperCase()|将字符串转成大写字符。|
|substring()|截取字符串。|
|equals()|字符串比较。|


#### 抽象类必须要有抽象方法吗？
不需要，抽象类不一定非要有抽象方法。


```java
abstract class Cat {
    public static void sayHi() {
        System.out.println("hi~");
    }
}
```


上面代码，抽象类并没有抽象方法但完全可以正常运行。


#### 普通类和抽象类有哪些区别？
1. 普通类不能包含抽象方法，抽象类可以包含抽象方法。
2. 抽象类不能直接实例化，普通类可以直接实例化。


#### 抽象类能使用final修饰吗？
不能，定义抽象类就是让其他类继承的，如果定义为final该类就不能被继承，这样彼此就会产生矛盾，所以final不能修饰抽象类。


#### 接口和抽象类有什么区别？
|区别|接口|抽象类|
| :----- | :----- | :----- |
|实现|使用implements来实现接口。|子类使用extends来继承。|
|构造函数|不能有。|可以有构造函数。|
|实现数量|类可以实现很多个接口。|只能继承一个抽象类。|
|访问修饰符|方法默认使用public修饰。|可以是任意访问修饰符。|


#### Java中IO流分为几种？
1. 按功能来分: 输入流(input)、输出流(output)。
2. 按类型来分: 字节流和字符流。


字节流和字符流的区别是，字节流按8位传输以字节为单位输入输出数据，字符流按16位传输以字符为单位输入输出数据。


#### BIO、NIO、AIO有什么区别？
|类型|解释|
| :----- | :----- |
|<div style='width: 60px'>BIO</div>|Block IO同步阻塞式IO，就是我们平常使用的传统IO，它的特点是模式简单使用方便，并发处理能力低。|
|<div style='width: 60px'>NIO</div>|New IO(或Non Blocking IO)同步非阻塞IO，是传统IO的升级，客户端和服务器端通过Channel(通道)通讯，实现了多路复用。|
|<div style='width: 60px'>AIO</div>|Asynchronous IO是NIO的升级，也叫NIO2，实现了异步非堵塞IO，异步IO的操作基于事件和回调机制。|


#### Files的常用方法都有哪些？
|方法|解释|
| :----- | :----- |
|Files.exists()|检测文件路径是否存在。|
|Files.createFile()|创建文件。|
|Files.createDirectory()|创建文件夹。|
|Files.delete()|删除一个文件或目录。|
|Files.copy()|复制文件。|
|Files.move()|移动文件。|
|Files.size()|查看文件个数。|
|Files.read()|读取文件。|
|Files.write()|写入文件。|


### 容器


#### Java容器都有哪些？
![](/images/ReviewVI/Collection.png)


#### Collection和Collections有什么区别？
| 类 | 解释 |
| :----- | :----- | 
|Collection|是一个集合接口，它提供了对集合对象进行基本操作的通用接口方法，所有集合都是它的子类，比如List、Set等。|
|Collections|是一个包装类，包含了很多静态方法，不能被实例化，就像一个工具类，比如提供的排序方法Collections.sort(list)。|


#### List、Set、Map之间的区别是什么？
![](/images/ReviewVI/listmapset.png)


#### HashMap和Hashtable有什么区别？
// todo


#### 如何决定使用HashMap还是TreeMap？
// todo


#### 说一下HashMap的实现原理？
// todo


#### 说一下HashSet的实现原理？
// todo


#### ArrayList和LinkedList的区别是什么？
// todo


#### 如何实现数组和List之间的转换？
// todo


#### ArrayList和Vector的区别是什么？
// todo


#### Array和ArrayList有何区别？
// todo


#### 在Queue中poll()和remove()有什么区别？
// todo


#### 哪些集合类是线程安全的？
// todo


#### 迭代器Iterator是什么？
// todo


#### Iterator怎么使用？有什么特点？
// todo


#### Iterator和ListIterator有什么区别？
// todo


#### 怎么确保一个集合不能被修改？
// todo


### 多线程


#### 并行和并发有什么区别？
// todo


#### 线程和进程的区别？
// todo


#### 守护线程是什么？
// todo


#### 创建线程有哪几种方式？
// todo


#### 说一下runnable和callable有什么区别？
// todo


#### 线程有哪些状态？
// todo


#### sleep()和wait()有什么区别？
// todo


#### notify()和notifyAll()有什么区别？
// todo


#### 线程的run()和start()有什么区别？
// todo


#### 创建线程池有哪几种方式？
// todo


#### 线程池都有哪些状态？
// todo


#### 线程池中submit()和execute()方法有什么区别？
// todo


#### 在Java程序中怎么保证多线程的运行安全？
// todo


#### 多线程中synchronized锁升级的原理是什么？
在锁对象的对象头里面有一个threadid字段，在第一次访问的时候threadid为空，jvm让其持有偏向锁，并将threadid设置为其线程id，再次进入的时候会先判断threadid是否与其线程id一致，如果一致则可以直接使用此对象；如果不一致，则升级偏向锁为轻量级锁，通过自旋循环一定次数来获取锁，执行一定次数之后，如果还没有正常获取到要使用的对象，此时就会把锁从轻量级升级为重量级锁，此过程就构成了synchronized锁的升级。


锁升级是为了减低了锁带来的性能消耗。在Java 6之后优化synchronized的实现方式，使用了偏向锁升级为轻量级锁再升级到重量级锁的方式，从而减低了锁带来的性能消耗。


#### 什么是死锁？
// todo


#### 怎么防止死锁？
// todo


#### ThreadLocal是什么？有哪些使用场景？
// todo


#### 说一下synchronized底层实现原理？
// todo


#### synchronized和volatile的区别是什么？
// todo


#### synchronized和Lock有什么区别？
// todo


#### synchronized和ReentrantLock区别是什么？
// todo


#### 说一下atomic的原理？
通过CAS乐观锁保证原子性，通过自旋保证当次修改的最终修改成功，通过降低锁粒度(多段锁)增加并发性能。


### 反射


#### 什么是反射？
// todo


#### 什么是Java序列化？什么情况下需要序列化？
// todo


#### 动态代理是什么？有哪些应用？
// todo


#### 怎么实现动态代理？
// todo


### 对象拷贝


#### 为什么要使用克隆？
// todo


#### 如何实现对象克隆？
// todo


#### 深拷贝和浅拷贝区别是什么？
// todo


### Java Web模块


#### JSP和servlet有什么区别？
// todo


#### JSP有哪些内置对象？作用分别是什么？
// todo


#### 说一下JSP的4种作用域？
// todo


#### session和cookie有什么区别？
// todo


#### 说一下session的工作原理？
// todo


#### 如果客户端禁止cookie能实现session还能用吗？
// todo


#### springMVC和struts的区别是什么？
// todo


#### 如何避免sql注入？
// todo


#### 什么是XSS攻击？如何避免？
// todo


#### 什么是CSRF攻击？如何避免？
// todo


### 异常模块


#### throw和throws的区别？
// todo


#### final、finally、finalize有什么区别？
// todo


#### try-catch-finally中哪个部分可以省略？
// todo


#### try-catch-finally中，如果catch中return了，finally还会执行吗？
// todo


#### 常见的异常类有哪些？
// todo


### 网络模块


#### http响应码301和302代表的是什么？有什么区别？
// todo


#### forward和redirect的区别？
// todo


#### 简述TCP和UDP的区别？
// todo


#### TCP为什么要三次握手，两次不行吗？为什么？
// todo


#### 说一下TCP粘包是怎么产生的？
// todo


#### OSI的七层模型都有哪些？
// todo


#### get和post请求有哪些区别？
// todo


#### 如何实现跨域？
// todo


#### 说一下JSONP实现原理？
// todo


### 设计模式


#### 说一下你熟悉的设计模式？
// todo


#### 简单工厂和抽象工厂有什么区别？
// todo


### Spring MVC


#### 为什么要使用Spring？
// todo


#### 解释一下什么是AOP？
// todo


#### 解释一下什么是IOC？
// todo


#### Spring有哪些主要模块？
// todo


#### Spring常用的注入方式有哪些？
// todo


#### Spring中的bean是线程安全的吗？
// todo


#### Spring支持几种bean的作用域？
// todo


#### Spring自动装配bean有哪些方式？
// todo


#### Spring事务实现方式有哪些？
// todo


#### 说一下Spring的事务隔离？
// todo


#### 说一下SpringMVC运行流程？
// todo


#### SpringMVC有哪些组件？
// todo


#### @RequestMapping的作用是什么？
// todo


#### @Autowired的作用是什么？
// todo


### Spring Boot/Spring Cloud


#### 什么是Spring Boot？
// todo


#### 为什么要用Spring Boot？
// todo


#### Spring Boot核心配置文件是什么？
// todo


#### Spring Boot配置文件有哪几种类型？它们有什么区别？
// todo


#### Spring Boot有哪些方式可以实现热部署？
// todo


#### JPA和Hibernate有什么区别？
// todo


#### 什么是Spring Cloud？
// todo


#### Spring Cloud断路器的作用是什么？
// todo


#### Spring Cloud的核心组件有哪些？
// todo


### Hibernate


#### 为什么要使用Hibernate？
// todo


#### 什么是ORM框架？
// todo


#### Hibernate中如何在控制台查看打印的sql语句？
// todo


#### Hibernate有几种查询方式？
// todo


#### Hibernate实体类可以被定义为final吗？
// todo


#### 在Hibernate中使用Integer和int做映射有什么区别？
// todo


#### hibernate是如何工作的？
// todo


#### get()和load()的区别？
// todo


#### 说一下Hibernate的缓存机制？
// todo


#### Hibernate对象有哪些状态？
// todo


#### 在Hibernate中getCurrentSession和openSession的区别是什么？
// todo


#### hibernate实体类必须要有无参构造函数吗？为什么？
// todo


### Mybatis


#### Mybatis中#{}和${}的区别是什么？
// todo


#### Mybatis有几种分页方式？
// todo


#### RowBounds是一次性查询全部结果吗？为什么？
// todo


#### Mybatis逻辑分页和物理分页的区别是什么？
// todo


#### Mybatis是否支持延迟加载？延迟加载的原理是什么？
// todo


#### 说一下Mybatis的一级缓存和二级缓存？
// todo


#### Mybatis和Hibernate的区别有哪些？
// todo


#### Mybatis有哪些执行器(Executor)？
// todo


#### Mybatis分页插件的实现原理是什么？
// todo


#### Mybatis如何编写一个自定义插件？
// todo


### RabbitMQ


#### RabbitMQ的使用场景有哪些？
// todo


#### RabbitMQ有哪些重要的角色？
// todo


#### RabbitMQ有哪些重要的组件？
// todo


#### RabbitMQ中vhost的作用是什么？
// todo


#### RabbitMQ的消息是怎么发送的？
// todo


#### RabbitMQ怎么保证消息的稳定性？
// todo


#### RabbitMQ怎么避免消息丢失？
// todo


#### 要保证消息持久化成功的条件有哪些？
// todo


#### RabbitMQ持久化有什么缺点？
// todo


#### RabbitMQ有几种广播类型？
// todo


#### RabbitMQ怎么实现延迟消息队列？
// todo


#### RabbitMQ集群有什么用？
// todo


#### RabbitMQ节点的类型有哪些？
// todo


#### RabbitMQ集群搭建需要注意哪些问题？
// todo


#### RabbitMQ每个节点是其他节点的完整拷贝吗？为什么？
// todo


#### RabbitMQ集群中唯一一个磁盘节点崩溃了会发生什么情况？
// todo


#### RabbitMQ对集群节点停止顺序有要求吗？
// todo


### Kafka


#### Kafka可以脱离zookeeper单独使用吗？为什么？
// todo


#### Kafka有几种数据保留的策略？
// todo


#### Kafka同时设置了7天和10G清除数据，到第五天的时候消息达到了10G，这个时候Kafka将如何处理？
// todo


#### 什么情况会导致Kafka运行变慢？
// todo


#### 使用Kafka集群需要注意什么？
// todo


### Zookeeper


#### Zookeeper是什么？
// todo


#### Zookeeper都有哪些功能？
// todo


#### Zookeeper有几种部署模式？
// todo


#### Zookeeper怎么保证主从节点的状态同步？
// todo


#### 集群中为什么要有主节点？
// todo


#### 集群中有3台服务器，其中一个节点宕机，这个时候Zookeeper还可以使用吗？
// todo


#### 说一下Zookeeper的通知机制？
// todo


####
// todo


### Mysql


#### 数据库的三范式是什么？
// todo


#### 一张自增表里面总共有7条数据，删除了最后2条数据，重启Mysql数据库，又插入了一条数据，此时ID是多少？
// todo


#### 如何获取当前数据库版本？
// todo


#### 说一下ACID是什么？
// todo


#### char和varchar的区别是什么？
// todo


#### float和double的区别是什么？
// todo


#### Mysql的内连接、左连接、右连接有什么区别？
// todo


#### Mysql索引是怎么实现的？
// todo


#### 怎么验证Mysql的索引是否满足需求？
// todo


#### 说一下数据库的事务隔离？
// todo


#### 说一下Mysql常用的引擎？
// todo


#### 说一下Mysql的行锁和表锁？
// todo


#### 说一下乐观锁和悲观锁？
// todo


#### Mysql问题排查都有哪些手段？
// todo


#### 如何做Mysql的性能优化？
// todo


### Redis


#### Redis是什么？都有哪些使用场景？
// todo


#### Redis有哪些功能？
// todo


#### Redis和memcache有什么区别？
// todo


#### Redis为什么是单线程的？
// todo


#### 什么是缓存穿透？怎么解决？
// todo


#### Redis支持的数据类型有哪些？
// todo


#### Redis支持的Java客户端都有哪些？
// todo


#### jedis和redisson有哪些区别？
// todo


#### 怎么保证缓存和数据库数据的一致性？
// todo


#### Redis持久化有几种方式？
// todo


#### Redis怎么实现分布式锁？
// todo


#### Redis分布式锁有什么缺陷？
// todo


#### Redis如何做内存优化？
// todo


#### Redis淘汰策略有哪些？
// todo


#### Redis常见的性能问题有哪些？该如何解决？
// todo


### JVM


#### 说一下JVM的主要组成部分？及其作用？
// todo


#### 说一下JVM运行时数据区？
// todo


#### 说一下堆栈的区别？
// todo


#### 队列和栈是什么？有什么区别？
// todo


#### 什么是双亲委派模型？
// todo


#### 说一下类装载的执行过程？
// todo


#### 怎么判断对象是否可以被回收？
// todo


#### Java中都有哪些引用类型？
// todo


#### 说一下JVM有哪些垃圾回收算法？
// todo


#### 说一下JVM有哪些垃圾回收器？
// todo


#### 详细介绍一下CMS垃圾回收器？
// todo


#### 新生代垃圾回收器和老生代垃圾回收器都有哪些？有什么区别？
// todo


#### 简述分代垃圾回收器是怎么工作的？
// todo


#### 说一下JVM调优的工具？
// todo


#### 常用的JVM调优的参数都有哪些？
// todo
