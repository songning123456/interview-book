## 题卷V


### 阿里巴巴


#### 对象如何进行深拷⻉，除了clone？
1. 构造函数
2. Apache Commons Lang序列化
3. Gson序列化


#### happen-before原则？
|happen-before原则|解释|
| :----- | :----- |
|单线程|在同一个线程中，书写在前面的操作happen-before后面的操作。|
|锁|同一个锁的unlock操作happen-before此锁的lock操作。|
|volatile|对一个volatile变量的写操作happen-before对此变量的任意操作(当然也包括写操作了)。|
|传递性|如果A操作happen-before B操作，B操作happen-before C操作，那么A操作happen-before C操作。|
|线程启动|同一个线程的start方法happen-before此线程的其它方法。|
|线程中断|对线程interrupt方法的调用happen-before被中断线程的检测到中断发送的代码。|
|线程终结|线程中的所有操作都happen-before线程的终止检测。|
|对象创建|一个对象的初始化完成先于他的finalize方法调用。|


#### JVM调优的实践？
// todo


#### 单例对象会被JVM的GC时回收吗？
<span style="color: red">不会</span>


**判断是否可回收**


以主流的JVM HotSpot来说，使用的是可达性分析，通过从GC Roots的引用作为起点，来判断对象可达性，从而决定是否回收该对象。那什么可以作为GC Roots呢？


1. 虚拟机栈(栈桢中的本地变量表)中的引用的对象。
2. 方法区中的类静态属性引用的对象。
3. 方法区中的常量引用的对象。
4. 本地方法栈中JNI的引用的对象。


从单例模式创建的对象来看，可以判断其符合方法区中的类静态属性引用的对象这条定义。方法区中的类中的静态属性必然引用堆中的对象。那如此看，如果没办法解决方法区中的类的话，可以认为单例对象不会被回收。类会放在方法区，对象会放在堆中。


**判断方法区中的类**


再想深一层，什么情况下有可能触发方法区(JDK8-)或元数据区(JDK8+)中单例类消亡？这样的话，堆中的对象就不可达，从而会被回收。如果这个假想不现实，则可认为单例模式实现的对象不可回收。符合下列三点则可认为类卸载:


1. 该类所有的实例都已经被回收，也就是Java堆中不存在该类的任何实例。
2. 加载该类的Class Loader已经被回收。
3. 该类对应的java.lang.Class对象没有任何地方被引用，无法在任何地方通过反射访问该类的方法。


要求相当苛刻，所以一般情况下可以认定不可回收了。


#### Redis如果list较大，怎么优化？
redis list分片，当redis的list数据量比较大时采用分片处理。


#### TCP的粘包与半包？
1. 发送方和接收方规定固定大小的缓冲区，也就是发送和接收都使用固定大小的byte[]数组长度，当字符长度不够时使用空字符弥补；
2. 在TCP协议的基础上封装一层数据请求协议，既将数据包封装成`数据头(存储数据正文大小)+数据正文`的形式，这样在服务端就可以知道每个数据包的具体长度了，知道了发送数据的具体边界之后，就可以解决半包和粘包的问题了；
3. 以特殊的字符结尾，比如以“\n”结尾，这样我们就知道结束字符，从而避免了半包和粘包问题(推荐解决方案)。


👉 [TCP粘包和半包问题及解决](https://blog.csdn.net/qq_45915957/article/details/116001523)


#### socket编程相关的一些API和用法？
* DatagramSocket


它是用来描述一个socket对象的，对socket文件进行了封装。有两个核心方法:


1. receive(): 接受数据，如果没有数据过来，receive就会阻塞等待。如果有数据过来了，receive就会返回一个DatagramPacket对象。
2. send(): 以DatagramPacket为单位进行发送。


* DatagramPacket


它是用来描述一个UDP数据报的，对UDP数据报进行了封装。面向数据报，就是以DatagramPacket为单位进行的。注: 发送的时候，要知道发送的目标在哪；接受的时候，需要知道这个数据是从哪里来。因此就涉及到IP地址和端口号。JDK都将它们封装成InetSocketAddress类来表示。


#### 建立和处理连接的是同一个socket吗？socket中两个队列分别是啥？
TCP三次握手建立连接的过程中，内核通常会为每一个LISTEN状态的Socket维护两个队列:


1. SYN队列(半连接队列): 这些连接已经接到客户端SYN；
2. ACCEPT队列(全连接队列): 这些连接已经接到客户端的ACK，完成了三次握手，等待被accept系统调用取走。


👉 [计算机网络](https://blog.csdn.net/weixin_44478659/article/details/121035192)


####  项目中有使用过Netty吗？
// todo


#### TSL1.3新特性？
// todo


#### AES算法原理
![](/images/ReviewIV/AES.png)


#### Redis集群的使用
// todo


#### mysql与mogo对比
// todo


#### 场景题: 设计一个im系统包括群聊单聊
// todo


#### 场景题: 设计数据库连接池
// todo


#### 场景题: 秒杀场景的设计
// todo


### 美团


#### 项目详细信息，涉及一些AIOT交互处理，怎么实现大量的不同设备的指令编解码和指令转化，服务器的架构，自己责任模块
// todo


#### OOM的故障处理
// todo


#### 有没有用过分布式锁，怎么实现的，讲讲原理
// todo


#### Redis的跳表用在哪，为什么用跳表
跳跃表以有序的方式在层次化的链表中保存元素，在大多数情况下，跳跃表的效率可以和平衡树媲美，查找、删除、添加等操作都可以在对数期望时间下完成，并且比起平衡树来说，跳跃表的实现要简单直观得多。所以在Redis中没有使用平衡树，而是使用了跳跃表。


跳跃表的结构是多层的，通过从最高维度的表进行检索再逐渐降低维度从而达到对任何元素的检索接近线性时间的目的O(logn)。理想的跳表是每一层是下一层元素的1/2，即每个元素跳过2个元素，这样共有log2N层。但是这样插入删除元素就会很复杂，ex插入一个元素需要更新所有层相关的节点。所以通常的做法：没次向跳表加入一个元素时，用扔硬币的方式决定要不要向上增长一层。


![](/images/ReviewIV/skiplist.png)


👉 [5分钟了解Redis的内部实现跳跃表](https://blog.csdn.net/sinat_19594515/article/details/118864717)


#### Mysql优化的实践经验
// todo


#### HashMap的1.8与1.7区别
// todo


#### Netty的原理和使用
// todo


#### TCP的连接过程
// todo


#### Socket有几个队列
// todo


#### 一台服务器能支持多少连接，为什么？


实际情况下，每创建一个链接需要消耗一定的内存，大概是4-10kb，所以链接数也受限于机器的总内存。(链接发起端，活力全开才64000左右链接，内存最多才占用640M，一般客户端都能满足；内存限制主要还是考虑服务器端。)


👉 [单台服务器最大支持多少连接数](https://blog.csdn.net/wangpeng322/article/details/99842126)


👉 [一台服务器能支撑多少个TCP连接](http://t.zoukankan.com/lizexiong-p-14528874.html)


#### TCP各个参数怎么设置
👉 [TCP参数设置](http://t.zoukankan.com/DengGao-p-tcp_parameter.html)


#### Redis底层基本数据类型，redis集群原理，cluster集群的使用
// todo


#### Mysql存储引擎类型、索引类型，Innodb数据存储方式
// todo


#### 线程池的参数说明，rejectHandler说明
```java
public ThreadPoolExecutor(int corePoolSize, 
                          int maximumPoolSize,
                          long keepAliveTime, TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler handler)
```


👉 [线程池参数及使用说明](https://blog.csdn.net/changlina_1989/article/details/110630674)


#### volatile的原理


#### JVM有哪几种垃圾回收器，各自的应用场景


#### G1回收器的特征


#### JVM结构


#### 负载均衡器的四层和七层负载均衡原理


#### 场景题: 设计一个高可用高并发的电商系统


### 腾讯


#### kafka生产端怎么实现幂等的


#### kafka如何实现分布式消息


#### kafka的slave的同步机制


#### kafka怎么进行消息写入的ack


#### 为什么实现equals必须先实现hash方法


#### 一个对象new出来后的结构，怎么保存的


#### 讲一讲类加载的过程


#### Redis的hash数据结构和如何扩容


#### mysql快照读怎么实现的


#### mysql的事务隔离级别，不可重复读和幻读区别


### YY


#### JVM调优思路


#### Redis cluster集群扩容怎么数据平滑过度，从客户端设计


#### Mysql的sql本身没问题的情况下，没走索引原因？(反复强调sql没问题，不需要从sql⻆度考虑)


👉 [为什么mysql没走索引？](https://blog.csdn.net/llchopin/article/details/118675176)


#### kafka如何确保消息不丢失


#### 分库分表如何进行跨库联合查询


#### 限流设计用JAVA实现，不能用工具类库


#### Dubbo的设计和完整调用过程(要详细)


#### ES的脑裂问题怎么解决


### 得物


#### new一个对象的过程发生了什么


#### Spring循环引用解决的原理是什么？


#### FactoryBean和BeanFactory区别


#### Synchronized原理？


#### CAS volatile原理？


#### 内存模型？什么是主内存？什么是工作内存？


#### 数据库索引类型？原理？


#### Spring Bean生命周期？


#### mysql优化经验？


#### mysql锁类型？


#### Redis使用过程中应该注意什么问题？


#### JVM调优参数？


#### 线程池原理？属性代表含义？


#### HashMap、ConcurrentHashMap原理？


### 饿了么


#### 项目介绍，怎么不断优化项目、架构升级？如果业务量剧增，怎么保证系统高可用、扩展性？


#### 订单量、日新增多少？分库分表怎么做？基于什么维度去做？


#### 检测到jvm内存大于配置jvm的xmx配置的内存， 三台机器中的一台机器有上面这种现象，如何解释？


#### Redis热key怎么解决？


#### kafka为什么性能高？


#### OOM场景分析？


#### mysql集群是怎么部署的，主从同步？


#### 怎么设置使用什么GC方式？不同年代GC收集器有哪些？


#### 线上CPU很高怎么排查


#### jdk1.8的新特性


#### BIO、NIO了解


#### MQ怎么保证消息可靠性？


#### 系统负载过高怎么办、什么问题导致的？怎么排查？


#### Linux操作系统简单介绍有哪些东⻄？


### 中通


#### JVM介绍


#### JMM模型


#### GC Root有哪些？


#### JVM调优经验？


#### 线程池注意事项，异常处理


#### 分布式锁使用和原理？


#### Redis怎么持久化？高可用？


#### RPC框架实现原理？


#### 接口调用变慢排查


#### 业务系统架构，业务量


#### 数据库设计，优化方案


### ⻥泡泡(比心)


#### 比较有成就的项目


#### 清结算怎么实现的？


#### 统一收银台设计？


#### RocketMq和Kafka区别，选型？


#### kafka消息从生产到消费的流转过程？


#### HashMap、HashTable区别？


#### 对线程安全的理解？


#### CAS实现原理？


#### 代码加锁有几种实现方式？


#### 快速排序算法


#### 分布式锁获取锁失败的处理，线程间的同步？


#### Redis线程模型，过期机制，淘汰策略？


#### 线程池参数，使用场景，参数设置分析？


#### mysql存储引擎、索引结构、分库分表


#### 场景题: 设计一个抢红包系统
