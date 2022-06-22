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
1. 可见性
2. 有序性


👉 [深入理解volatile底层原理](https://blog.csdn.net/weixin_41951205/article/details/123193064)


#### JVM有哪几种垃圾回收器，各自的应用场景
JVM垃圾回收器是GC算法的具体实现。常用的垃圾回收器有以下几种，并且分别针对的是堆内存中的新生代、老年代。值得一提的是方法区(有的文章也将之称为永久代，主要存放类信息、常量池、静态类变量等)也会触发GC，主要是回收不可存活的常量对象以及无用类(当该类在程序中不再存在任何实例、对应的classloader被卸载、该类的类对象class不被任何地方引用，此时可以被回收)。


|JVM垃圾回收器|特性|
| :----- | :----- |
|Serial垃圾回收器|简要概括下该回收器的特点: <br>单线程，基于复制算法，JVM运行在Client时默认的新生代垃圾收集器。可以说Serial是最基本的垃圾回收器，使用的是复制算法，回收效率高，不容易产生不连续的内存碎片。并且也是单线程的垃圾回收器，只使用一个线程完成垃圾回收工作，并且此时会暂停其他工作线程。|
|ParNew垃圾回收器(Serial+多线程版本)|简要概括下该回收器的特点: <br>Serial的多线程实现，基于复制算法，JVM运行在Server时默认的新生代垃圾收集器。该垃圾回收器是Serial的多线程版本，采用的也是复制算法(毕竟都是作用于新生代嘛)，也是在执行垃圾回收收集时需要暂停其他工作线程。默认开启与CPU相同数目的线程数，可以通过参数-XX：ParallerGCThreads来修改默认开启的线程数量。|
|Parallel Scavenge垃圾回收器(多线程复制算法)|简要概括下该回收器的特点: <br>多线程，基于复制算法，以吞吐量最大化为目标，允许较长时间的STW换取吞吐量。该回收器也是新生代的GC回收器，同样采用复制算法，多线程的策略。主要关注的吞吐量(即CPU运行用户代码的时间/CPU总消耗的时间，吞吐量=运行用户代码时间/(运行用户代码时间+垃圾收集时间))。|
|Serial Old 垃圾回收器(单线程标记整理算法)|简单概括下该回收器的特点: <br>单线程，基于标记整理算法，是JVM运行在Client模式下默认的老年代垃圾回收器，可和Serial搭配使用。Serial Old回收器时Serial回收器的老年代版本，也是单线程，但采用的标记整理的算法。这里注意最重要的特点就是这两种回收器都会使得工作线程被停止，并且均是单线程模式。与两个Serial不同的是，Scavenge/ParNew采用的是多线程版本，但是此时也会暂停其他工作线程。|
|Parallel Old垃圾回收器(多线程标记整理算法)|简单概括下该垃圾回收器的特点: <br>多线程，基于标记整理算法，优先考虑系统的吞吐量。该垃圾回收器是Paraller Scavenge的老年代版本，多线程、标记整理。在JDK1.6之前，新生代使用ParallelScavenge收集器只能搭配年老代的Serial Old收集器，只能保证新生代的吞吐量优先，无法保证整体的吞吐量，Parallel Old正是为了在年老代同样提供吞吐量优先的垃圾收集器，如果系统对吞吐量要求比较高，可以优先考虑新生代Parallel Scavenge和年老代Parallel Old收集器的搭配策略。|
|CMS垃圾回收器(多线程标记清楚算法)|简单概括下该垃圾回收器的特点：<br>多线程，基于标记清除算法，为老年代设计，追求最短停顿时间。主要有四个步骤：初始标记、并发标记、重新标记、并发清除。不会暂停用户工作线程。全称为Concurrent mark sweep，该回收器是专门为老年代设计的，主要追求的是最短的停顿时间，采用的标记清楚算法。四个主要工作阶段的内容为: <br>1. 初始标记，标记一下GC Roots能直接关联的对象，速度很快，仍然需要暂停所有的工作线程。(这里简要回顾下哪些对象可以作为GC Roots: java虚拟机栈中的引用对象；方法区中的常量引用对象；方法区中的静态变量引用的对象；本地方法栈中一般Native方法引用的对象。)<br>2. 并发标记，进行GC Roots 跟踪的过程，和用户线程一起工作，不需要暂停工作线程。<br>3. 重新标记，为了修正在并发标记期间，因用户程序继续运行而导致标记产生变动的那一部分对象的标记记录，仍然需要暂停所有的工作线程。<br>4. 并发清除，清除GC Roots不可达对象，和用户线程一起工作，不需要暂停工作线程。由于耗时最长的并发标记和并发清除过程中，垃圾收集线程可以和用户现在一起并发工作，所以总体上来看CMS 收集器的内存回收和用户线程是一起并发地执行。|
|G1垃圾回收器|----|


#### G1回收器的特征
简要概括下该垃圾回收器的主要特点: 将堆内存分为几个大小固定的独立区域，在后台维护了一个优先列表，根据允许的收集时间回收垃圾收集价值最大的区域。相比CMS不会产生内存碎片，并且可精确控制停顿时间。分为四个阶段: 初始标记、并发标记、最终标记、筛选回收。


该垃圾回收器看似和CMS工作流程差不多。采用的却是标记清楚算法，但是本质上还是存在差别，尤其是讲堆内存分为大小固定的几个区域，并且维持了一个优先列表，选取其中最有价值的回收垃圾。


Garbage first垃圾收集器是目前垃圾收集器理论发展的最前沿成果，相比与CMS收集器，G1收集器两个最突出的改进是:


1. 基于标记-整理算法，不产生内存碎片。
2. 可以非常精确控制停顿时间，在不牺牲吞吐量前提下，实现低停顿垃圾回收。G1收集器避免全区域垃圾收集，它把堆内存划分为大小固定的几个独立区域，并且跟踪这些区域的垃圾收集进度，同时在后台维护一个优先级列表，每次根据所允许的收集时间，优先回收垃圾 最多的区域。区域划分和优先级区域回收机制，确保G1收集器可以在有限时间获得最高的垃圾收集效率。


#### JVM结构
// todo


#### 负载均衡器的四层和七层负载均衡原理
![](/images/ReviewV/LVSNginx.jpg)


|负载均衡|解释|
| :----- | :----- |
|<div style='width: 100px'>四层负载均衡</div>|四层的负载均衡就是基于IP+端口的负载均衡。<br>在三层负载均衡的基础上，通过发布三层的IP地址(VIP)，然后加四层的端口号，来决定哪些流量需要做负载均衡，对需要处理的流量进行NAT处理，转发至后台服务器，并记录下这个TCP或者UDP的流量是由哪台服务器处理的，后续这个连接的所有流量都同样转发到同一台服务器处理。对应的负载均衡器称为四层交换机(L4 switch)，主要分析IP层及TCP/UDP层，实现四层负载均衡。此种负载均衡器不理解应用协议(如HTTP/FTP/MySQL等等)，常见例子有LVS、F5。|
|<div style='width: 100px'>七层负载均衡</div>|七层的负载均衡就是基于虚拟的URL或主机IP的负载均衡。<br>在四层负载均衡的基础上(没有四层是绝对不可能有七层的)，再考虑应用层的特征，比如同一个Web服务器的负载均衡，除了根据VIP加80端口辨别是否需要处理的流量，还可根据七层的URL、浏览器类别、语言来决定是否要进行负载均衡。举个例子，如果你的Web服务器分成两组，一组是中文语言的，一组是英文语言的，那么七层负载均衡就可以当用户来访问你的域名时，自动辨别用户语言，然后选择对应的语言服务器组进行负载均衡处理。|


👉 [负载均衡之四层与七层](https://blog.csdn.net/w2009211777/article/details/123984559)


#### 场景题: 设计一个高可用高并发的电商系统
// todo


### 腾讯


#### Kafka生产端怎么实现幂等的
![](/images/ReviewV/KafkaEqual.png)


```
# 配置幂等性
props.put("enable.idempotence", true);
```


为了实现生产者的幂等性，Kafka引入了Producer ID(PID)和Sequence Number的概念。


1. 每个Producer在初始化时，都会分配一个唯一的PID，这个PID对用户来说是透明的。
2. 针对每个生产者(对应PID)发送到指定主题分区的消息都对应一个从0开始递增的Sequence Number。


👉 [Kafka如何保证幂等性](https://blog.csdn.net/bookssea/article/details/124562291)


#### Kafka如何实现分布式消息
// todo


#### Kafka的slave的同步机制
1. producer先从zookeeper的"/brokers/.../state"节点找到该partition的leader；
2. producer将消息发送给该leader；
3. leader将消息写入本地log；
4. followers从leader pull消息，写入本地log后leader发送ACK；
5. leader收到所有ISR中的replica的ACK后，增加HW(high watermark，最后commit的offset)并向producer发送ACK。


👉 [kafka_12_同步机制](https://blog.csdn.net/zxczb/article/details/107923247)


👉 [kafka消息与同步机制](https://blog.csdn.net/u010285974/article/details/83308554)


#### Kafka怎么进行消息写入的ack
Kafka的ack机制，指的是producer的消息发送确认机制，这直接影响到Kafka集群的吞吐量和消息可靠性。而吞吐量和可靠性就像硬币的两面，两者不可兼得，只能平衡。


|ack值|解释|
| :----- | :----- |
|ack=1|简单来说就是，producer只要收到一个分区副本成功写入的通知就认为推送消息成功了。这里有一个地方需要注意，这个副本必须是leader副本。只有leader副本成功写入了，producer才会认为消息发送成功。|
|ack=0|简单来说就是，producer发送一次就不再发送了，不管是否发送成功。|
|ack=-1|简单来说就是，producer只有收到分区内所有副本的成功写入的通知才认为推送消息成功了。|


#### 为什么实现equals()必须先实现hash()
对于对象集合的判重，如果一个集合含有100个对象实例，仅仅使用equals()方法的话，那么对于一个对象判重就需要比较4950次，随着集合规模的增大，时间开销是很大的。但是同时使用哈希表的话，就能快速定位到对象的大概存储位置，并且在定位到大概存储位置后，后续比较过程中，如果两个对象的hashCode不相同，也不再需要调用equals()方法，从而大大减少了equals()比较次数。所以从程序实现原理上来讲的话，既需要equals()方法，也需要hashCode()方法。那么既然重写了equals()，那么也要重写hashCode()方法，以保证两者之间的配合关系。


#### 一个对象new出来后的结构，怎么保存的
1. 堆内存是用来存放由new创建的对象和数组，即动态申请的内存都存放在堆内存；
2. 栈内存是用来存放在函数中定义的一些基本类型的变量和对象的引用变量。


例子: 局部变量存放在栈；new函数和malloc函数申请的内存在堆；函数调用参数，函数返回值，函数返回地址存放在栈。


|内存类型|解释|
| :----- | :----- |
|栈区(stack)|由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。|
|堆区(heap)|一般由程序员分配释放，若程序员不释放，程序结束时可能由OS回收。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表。|


#### 一个Java对象被new出来的全过程
1. 检查常量池中有没有一个符号代表这个类；
2. 检查这个类有没有被加载；
3. 在堆上或者TLAB上为对象分配内存空间；
4. 为对象的实例属性赋领值；
5. 设置对象头(Object_header)信息,比如对象是那个类的实例，对象的Hash码,GC分代年龄，锁状态标志,是否启用偏向锁等等；
6. 对象引用入栈。

    
#### 讲一讲类加载的过程
![](/images/ReviewV/ClassLoad.png)


👉 [简述类加载过程详解](https://blog.csdn.net/Y_eatMeat/article/details/122954134)


#### Redis的hash数据结构和如何扩容
**渐进式rehash**


在扩容和收缩的时候，如果哈希字典中有很多元素，一次性将这些键全部rehash到ht[1]的话，可能会导致服务器在一段时间内停止服务。所以，采用渐进式rehash的方式，详细步骤如下:


为ht[1]分配空间，让字典同时持有ht[0]和ht[1]两个哈希表。将rehashindex的值设置为0，表示rehash工作正式开始。在rehash期间，每次对字典执行增删改查操作是，程序除了执行指定的操作以外，还会顺带将ht[0]哈希表在rehashindex索引上的所有键值对rehash到ht[1]，当rehash工作完成以后，rehashindex的值+1。随着字典操作的不断执行，最终会在某一时间段上ht[0]的所有键值对都会被rehash到ht[1]，这时将rehashindex的值设置为-1，表示rehash操作结束。渐进式rehash采用的是一种分而治之的方式，将rehash的操作分摊在每一个的访问中，避免集中式rehash而带来的庞大计算量。需要注意的是在渐进式rehash的过程，如果有增删改查操作时，如果index大于rehashindex，访问ht[0]，否则访问ht[1]。


👉 [redis中hash扩容过程](https://blog.csdn.net/weixin_38004638/article/details/118206845)


#### Mysql快照读怎么实现的
Innodb存储引擎的快照读是基于多版本并发控制MVCC和undo log实现，通过MVCC机制提高系统读写并发性能，快照读只发生于select操作，但不包括select ... lock in share mode，select ... for update。


#### Mysql的事务隔离级别，不可重复读和幻读区别
// todo


### YY


#### JVM调优思路
// todo


#### Redis cluster集群扩容怎么数据平滑过度(从客户端设计)


👉 [Redis cluster集群扩容缩容原理](https://blog.csdn.net/hellozhxy/article/details/120855704)


#### 两个Redis集群，如何平滑数据迁移
1. 基于Redis自身的RDB/AOF备份机制。
2. 基于redis-dump导入导出json备份。
3. 基于redis-shake实现redis-cluster迁移。


👉 [面试官: 两个Redis集群，如何平滑数据迁移？](https://zhuanlan.zhihu.com/p/90445769)


#### Mysql的sql本身没问题的情况下，没走索引原因？(反复强调sql没问题，不需要从sql⻆度考虑)
当表的索引被查询，会使用最好的索引，除非优化器使用全表扫描更有效。优化器优化成全表扫描取决与使用最好索引查出来的数据是否超过表的30%的数据。


👉 [为什么mysql没走索引？](https://blog.csdn.net/llchopin/article/details/118675176)


#### Kafka如何确保消息不丢失
// todo


#### 分库分表如何进行跨库联合查询
可以使用第三方中间件来实现，比如mycat、shading-jdbc。


当客户端发送一条sql查询`select * from user;`此时中间件会根据有几个子表，拆分成多个语句`select * from user1; select * from user2; select * from user3;`等多条语句查询，然后将查询的结果返回给中间件，然后汇总给客户端。这些语句是并发执行的，所以效率会很高。


#### 限流设计用JAVA实现，不能用工具类库
限流顾名思义，就是对请求或并发数进行限制；通过对一个时间窗口内的请求量进行限制来保障系统的正常运行。如果我们的服务资源有限、处理能力有限，就需要对调用我们服务的上游请求进行限制，以防止自身服务由于资源耗尽而停止服务。


1. 阈值: 在一个单位时间内允许的请求量。如QPS限制为10，说明1秒内最多接受10次请求。
2. 拒绝策略: 超过阈值的请求的拒绝策略，常见的拒绝策略有直接拒绝、排队等待等。


👉 [java如何进行限流？](https://www.zhihu.com/question/482724391/answer/2389749712)


#### Dubbo的设计和完整调用过程(要详细)
// todo


#### ES的脑裂问题怎么解决
**什么是脑裂现象**


在ElasticSearch集群初始化或者主节点宕机的情况下，由候选主节点中选举其中⼀个作为主节点。指定候选主节点的配置为`node.master: true`。当主节点负载压⼒过⼤，或者集群环境中的⽹络问题，导致其他节点与主节点通讯的时候，主节点没来及响应，这样的话，某些节点就认为主节点宕机，重新选择新的主节点，这样的话整个集群的⼯作就有问题了，⽐如我们集群中有10个节点，其中7个候选主节点，1个候选主节点成为了主节点，这种情况是正常的情况。但是如果现在出现了我们上⾯所说的主节点响应不及时，导致其他某些节点认为主节点宕机⽽重选主节点，那就有问题了，这剩下的6个候选主节点可能有3个候选主节点去重选主节点，最后集群中就出现了两个主节点的情况，这种情况官⽅成为“脑裂现象”。集群中不同的节点对于master的选择出现了分歧，出现了多个master竞争，导致主分⽚和副本的识别也发⽣了分歧，把⼀些分歧中的分⽚标识为了坏⽚。


|脑裂问题成因|解释|
| :----- | :----- |
|⽹络问题|集群间的⽹络延迟导致⼀些节点访问不到master，认为master挂掉了从⽽选举出新的master，并对master上的分⽚和副本标红，分配新的主分⽚。|
|节点负载|主节点的角色既为master⼜为data，访问量较⼤时可能会导致ES停⽌响应造成⼤⾯积延迟，此时其他节点得不到主节点的响应认为主节点挂掉了，会重新选取主节点。|
|内存回收|data节点上的ES进程占⽤的内存较⼤，引发JVM的⼤规模内存回收，造成ES进程失去响应。|


|解决方案|解释|
| :----- | :----- |
|角色分离|即master节点与data节点分离，限制⾓⾊；数据节点时需要承担存储和搜索的工作的，压⼒会很⼤。所以如果该节点同时作为候选主节点和数据节点，那么⼀旦选上它作为主节点了，这时主节点的工作压力将会⾮常⼤，出现脑裂现象的概率就增加了。|
|减少误判|配置主节点的响应时间，在默认情况下，主节点3秒没有响应，其他节点就认为主节点宕机了，那我们可以把该时间设置得长⼀点，该配置是discovery.zen.ping_timeout:5。|
|选举触发|discovery.zen.minimum_master_nodes:1(默认是1)，该属性定义的是为了形成⼀个集群，有主节点资格并互相连接的节点的最⼩数⽬。⼀个有10节点的集群，且每个节点都有成为主节点的资格，discovery.zen.minimum_master_nodes参数设置为6。正常情况下，10个节点，互相连接，⼤于6，就可以形成⼀个集群。若某个时刻，其中有3个节点断开连接。剩下7个节点，⼤于6，继续运⾏之前的集群。⽽断开的3个节点，⼩于6，不能形成⼀个集群。该参数就是为了防⽌脑裂的产⽣建议设置为(候选主节点数/2)+1。|


👉 [ES脑裂问题分析及优化](https://blog.csdn.net/BruceLiu_code/article/details/110467660)


### 得物


#### new一个对象的过程发生了什么
![](/images/ReviewV/ClassLoad2.png)


1. 当虚拟机遇到一条new指令时，会去检查这个指令的参数能否在常量池中定位到一个类的符号引用，并检查代表的类是否已经被类加载器加载。如果没有被加载那么必须先执行这个类的加载。
2. 类加载检查通过后，虚拟机将为新对象分配内存，对象所需内存的大小在类加载后便可以确定。
3. 内存分配完成后，虚拟机需要将对象初始化为零值，保证对象的实例变量在代码中不赋初始值就能直接使用。类变量在类加载的准备阶段初始化为零值。
4. 对对象头进行必要信息的设置，比如如何找到类的元数据信息、对象的HashCode、GC分代年龄等。
5. 经过上述操作，一个新的对象已经产生，但是<init>方法还没有执行，所有的字段都是零值。这时候需要执行<init>方法(构造方法)把对象按照程序员的意愿进行初始化。类变量的初始化操作在类加载的初始化阶段<clinit>方法完成。


👉 [new一个对象的时候发生了什么？](https://blog.csdn.net/JavaShark/article/details/125300803)


#### Spring循环引用解决的原理是什么？
👉 [Spring循环依赖原理，如何解决？](https://baijiahao.baidu.com/s?id=1684246101101128706&wfr=spider&for=pc)


#### FactoryBean和BeanFactory区别
```java
public interface FactoryBean<T> {
	
    String OBJECT_TYPE_ATTRIBUTE = "factoryBeanObjectType";
    
	// 从factory中获取bean
	@Nullable
	T getObject() throws Exception;

	// 从beanFactory中获取类型
	@Nullable
	Class<?> getObjectType();

	// 是单例？
	default boolean isSingleton() {
		return true;
	}
    
}
```


```java
package com.example.demo.domian;

import org.springframework.beans.factory.FactoryBean;
import org.springframework.stereotype.Component;

@Component
public class MyFactoryBean implements FactoryBean {
	
    // 保存一句话，用来区分不同的对象。  
    private String message;
    // 无参构造器。
    public MyFactoryBean() {
        // 意思是：当前对象是 MyFactoryBean 的对象。
        this.message = "object of myFactoryBeanSelf";
    }
    // 有参构造器。
    public MyFactoryBean(String message) {
        this.message = message;
    }
    // 获取 message。
    public String getMessage() {
        return this.message;
    }
    
    @Override
    /**
    *  这个方法在执行时创建了新的 MyFactoryBean 类型的对象。
    *  这里继续沿用了 MyFactoryBean 类型，但是可以是别的类型
    *  比如：Person、Car、等等。
    */
    public Object getObject() throws Exception {
      // 意思是：当前对象是 MyFactoryBean 的 getObject() 创建的。
        return new MyFactoryBean("object from getObject() of MyFactoryBean");
    }
    
    @Override
    public Class<?> getObjectType() {
        return MyFactoryBean.class
    }
}
```


```java
package com.example.demo.domian;

import org.junit.jupiter.api.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.ApplicationContext;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = com.example.demo.domian.MyFactoryBean.class)
class MyFactoryBeanTest {
    @Autowired
    private ApplicationContext context;

    @Test
    public void test() {
        // 第一次用 myFactoryBean 去拿。  
        MyFactoryBean myBean1 = (MyFactoryBean) context.getBean("myFactoryBean");
        System.out.println("myBean1 = " + myBean1.getMessage());
        // 第二次用 &myFactoryBean 去拿。  
        MyFactoryBean myBean2 = (MyFactoryBean) context.getBean("&myFactoryBean");
        System.out.println("myBean2 = " + myBean2.getMessage());、
        // 判断两次拿到的对象是不是一样的？    
        System.out.println("myBean1.equals(myBean2) = " + myBean1.equals(myBean2));
    }
}
```


```
myBean1 = object from getObject() of MyFactoryBean
myBean2 = object of myFactoryBeanSelf
myBean1.equals(myBean2) = false
```


第一次使用myFactoryBean去容器中拿，实际上是容器中MyFactoryBean的bean调用了getObject()方法，并将结果返回。第二次使用&myFactoryBean去容器中拿，才是真正拿到了MyFactoryBean的bean。


👉 [BeanFactory和FactoryBean的区别](https://blog.csdn.net/yy_diego/article/details/115710104)


#### Synchronized原理？
```java
class Test1{
    public synchronized void test() {
    }
}

//等价于
class Test1{
    public void test() {
        // 锁的是当前对象
        synchronized(this) {
        }
    }
}
```


```java
class Test2{
    public synchronized static void test() {
    }
}
 
//等价于
class Test2{
    public static void test() {
        // 锁的是类对象，类对象只有一个
        synchronized(Test2.class) {
        }
    }
}
```


每个Java对象都可以关联一个Monitor对象，如果使用synchronized给对象上锁(重量级)之后，该对象头的Mark Word中就被设置指向Monitor对象的指针。不加synchronized的对象不会关联监视器。


![](/images/ReviewV/Monitor.png)


1. 刚开始Monitor中Owner为null。
2. 当Thread-2执行synchronized(obj)就会将Monitor的所有者Owner置为Thread-2，Monitor中只能有一个Owner。
3. 在Thread-2上锁的过程中，如果Thread-3、Thread-4、Thread-5也来执行synchronized(obj)，就会进入EntryList BLOCKED。
4. Thread-2执行完同步代码块的内容，然后唤醒EntryList中等待的线程来竞争锁，竞争的时是非公平的。
5. 图中WaitSet中的Thread-0、Thread-1是之前获得过锁，但条件不满足进入WAITING状态的线程。


👉 [synchronized原理](https://blog.csdn.net/qq_34416191/article/details/119714263)


#### CAS volatile原理？


#### 内存模型？什么是主内存？什么是工作内存？


#### 数据库索引类型？原理？


#### Spring Bean生命周期？


#### Mysql优化经验？


#### Mysql锁类型？


#### Redis使用过程中应该注意什么问题？


#### JVM调优参数？


#### 线程池原理？属性代表含义？


#### HashMap、ConcurrentHashMap原理？


### 饿了么


#### 项目介绍，怎么不断优化项目、架构升级？如果业务量剧增，怎么保证系统高可用、扩展性？


#### 订单量、日新增多少？分库分表怎么做？基于什么维度去做？


#### 检测到JVM内存大于配置JVM的xmx配置的内存， 三台机器中的一台机器有上面这种现象，如何解释？


#### Redis热key怎么解决？


#### Kafka为什么性能高？


#### OOM场景分析？
![](/images/ReviewV/OOM.jpg)


1. StackOverflowError
2. Java heap space
    1. 内存溢出，是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory；比如申请了一个Integer，但给它存了Long才能存下的数，那就是内存溢出。
    2. 内存泄露，是指程序在申请内存后，无法释放已申请的内存空间，一次内存泄露危害可以忽略，但内存泄露堆积后果很严重，无论多少内存，迟早会被占光。
3. GC overhead limit exceeded
4. Direct buffer memory
5. Unable to create new native thread
6. Metaspace
7. Requested array size exceeds VM limit
8. Out of swap space
9. Kill process or sacrifice child


👉 [java 9种常见的OOM场景——原因分析及解决方案](https://blog.csdn.net/zhangkaixuan456/article/details/111904430)


#### Mysql集群是怎么部署的，主从同步？
从mysql和主mysql的配置基本相同,除了以下几个配置: 


1. 从机server-id要和主机不同
    1. server-id=2
    2. log_slave_updates=1
2. 开启同步函数
    3. log_bin_trust_function_creators=1
    
    
#### Mysql双主相互备份是如何解决循环复制的？
假设A、B相互备份，A产生的binlog中携带serverId=1发送到B，B解析后执行，执行完毕也会产生binlog。此时B的binlog serverId仍然等于1，发送给A后，A看到serverId=1为自己发送的binlog，直接丢弃；同理B方向类似。


#### 怎么设置使用什么GC方式？不同年代GC收集器有哪些？


#### 线上CPU很高怎么排查


👉 [记录一次线上CPU负载过高的排查过程](https://blog.csdn.net/I_peter/article/details/123543297)


#### JDK1.8的新特性


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


#### Kafka消息从生产到消费的流转过程？


#### HashMap、HashTable区别？


#### 对线程安全的理解？


#### CAS实现原理？


#### 代码加锁有几种实现方式？


#### 快速排序算法


#### 分布式锁获取锁失败的处理，线程间的同步？


#### Redis线程模型，过期机制，淘汰策略？


#### 线程池参数，使用场景，参数设置分析？


#### Mysql存储引擎、索引结构、分库分表


#### 场景题: 设计一个抢红包系统
