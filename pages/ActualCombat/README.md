#### 自我介绍
```
    我叫sonin，今年25岁,毕业于南京工程学院计算机科学与技术专业。从事Java + Vue的
开发,目前已有两年工作经验。拥有较为扎实的基础知识，良好的编程风格。熟悉SpringBoot 
+ Vue-Cli模式的Web开发。了解Spring，SpringMVC等常用开源框架以及Mysql，Redis等常用数
据库，同时熟悉跨平台的开源框架UniApp。
    利用业余时间部署过个人的开源项目，所以对于项目从开发到部署的整个流程都有一定的
了解。但我仍觉得目前经历甚浅，希望更多的了解社会与软件的需求关系，通过更多的项目实
践提高自身的软件设计能力和编程技术。
```



#### 抽象类和接口有什么区别?

###### 抽象类
* 抽象方法只作声明，而不包含实现，可以看成是没有实现体的虚方法。
* 抽象类不能被实例化。
* 抽象类可以但不是必须有抽象属性和抽象方法，但是一旦有了抽象方法，就一定要把这个类声明为抽象类。
* 具体派生类必须覆盖基类的抽象方法。
* 抽象派生类可以覆盖基类的抽象方法，也可以不覆盖。如果不覆盖，则其具体派生类必须覆盖它们。
  

###### 接口
* 抽象类可以有构造方法，接口中不能有构造方法。
* 抽象类中可以有普通成员变量，接口中没有普通成员变量。
* 抽象类中可以包含静态方法，接口中不能包含静态方法。
* 一个类可以实现多个接口，但只能继承一个抽象类。
* 接口可以被多重实现，抽象类只能被单一继承。
* 如果抽象类实现接口，则可以把接口中方法映射到抽象类中作为抽象方法而不必实现，而在抽象类的子类中实现接口中方法。


###### 抽象类和接口的相同点
* 都可以被继承；
* 都不能被实例化；
* 都可以包含方法声明；
* 派生类必须实现未实现的方法。


###### 抽象类和接口的不同点
* 抽象类可以有构造方法，接口中不能有构造方法。
* 抽象类中可以有普通成员变量，接口中没有普通成员变量。
* 抽象类中可以包含静态方法，接口中不能包含静态方法。
* 一个类可以实现多个接口，但只能继承一个抽象类。
* 接口可以被多重实现，抽象类只能被单一继承。
* 如果抽象类实现接口，则可以把接口中方法映射到抽象类中作为抽象方法而不必实现，而在抽象类的子类中实现接口中方法。



#### 线程池有哪些核心参数?
* corePoolSize
<div style="text-indent:2em">表示常驻核心线程数量。</div>
* maximumPoolSize
<div style="text-indent:2em">表示线程池中能同时执行的最大线程数量。这个值必须大于等于corePoolSize，如果这两个值相等，那就是固定大小的线程池。</div>
* keepAliveTime
<div style="text-indent:2em">表示线程池中除常驻核心线程之外的其他线程的空闲时间，如果超过这个时间就会销毁。</div>
* queue
<div style="text-indent:2em">缓存队列，当请求的线程数大于corePoolSize的时候，线程会进入队列进行阻塞。当这个队列达到上限之后，线程池会创建新的线程，直到到吗maximumPoolSize大小位置。</div>
* RejectedExecutionHandler
<div style="text-indent:2em">表示拒绝策略。当queue满了之后，并行活动的线程数大于maximunPoolSize的时候，线程池通过改策略处理请求。</div>



#### 线程池的拒绝策略?
名称 | 定义
:-: | :-:
AbortPolicy(默认) | 丢弃这个任务并抛出RejectedExecutionException异常
DiscardPolicy | 丢弃掉这个任务，但是不抛出异常
DiscardOldestPolicy | 抛弃掉在队列中等待最久的任务，然后把当前任务加入队列中
CallerRunsPolicy | 调用任务的run()方法绕过线程池直接执行


```
// 自定义策略
public class UserRejectedHandler implements RejectedExecutionHandler {
    @Override
    public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
        System.out.println("处理逻辑");
    }
}
```



#### 你知道java有哪些锁机制?说说他们的区别?以及使用场景?

###### 区别
区别/名称 | synchronized | Lock
:-: | :-: | :-:
原始构成 | synchronized是关键字属于jvm层面。monitorenter(底层是通过monitor对象来完成的，其实wait/notify等方法也依赖于monitor对象，只有在同步块或方法中才能调用wait/notify等方法) | Lock是具体类（java.util.concurrent.locks.lock）是api层面的锁。
使用方法 | synchronized不需要用户去手动释放锁，当synchronized代码执行完后自动让线程释放对锁的占用！| Reentrantlock则需要用户手动释放锁，就有可能出现死锁现象。需要lock()和unlock()方法配合try/finally语句块来完成。
等待是否可中断 | synchronized不可中断，除非抛出异常或者正常完成。| Reentrantlock可中断: 1.设置超时方法 tryLock(long timeout,TimeUnit unit) 2.lockInterruptibly()放代码块中，调用interrupt()方法可中断。
加锁是否公平 | synchronized非公平锁。 | Reentrantlock 两者都可以，默认非公平锁。
锁绑定多个条件Condition | synchronized没有。| ReentrantLock用来实现分组唤醒需要唤醒的线程，可以精确唤醒，而不是像synchronized要么随机唤醒一个要么唤醒全部线程。



###### synchronized的使用场景
* synchronized加在方法上，或者锁定类实例
```
public class Test{
        public synchronized void testMethod1(){}
        public void testMethod2(){
            sysnchronized(this){}
        }
}
```
这两种情况锁的都是类的实例对象，如果Test testObj1=new Test();现在两个线程thread1,thread2，如果thread1访问testObj1.test1或testObj1.test2那么thread2是无法同时访问的，因为testObj1对象的锁已经被thread1使用了，只有thread1释放锁之后才能被thread2占用；如果再有一个对象Test testObj2=new test();此时thread1访问testObj1.testMethod1方法，thread2访问testObj2.testMethod1方法是互不影响的。


* synchronized加在对象上
```
public class Test{
        Object a =new Object();
        //String a="";
        public void test(){
                synchronized(a){}
        }
}
```
此种方式我认为锁的是内存地址。


* synchronized加在静态变量或类实例，或在静态方法上
```
public class Test{
        static Object o=new Object();
        public static void test1(){
                synchronized(o){}
        }
        public static synchronized void test2(){}
        public static void test3(){
                synchronized(Test.class){}
        }
}
```
这3种情况锁的是Test类，而非Test的对象，只要有一个thread访问了该类的方法，其它thread一律等待。



#### 死锁产生的原因以及解决办法?

###### 原因
<div style="text-indent:2em">当一个线程永远地持有一个锁，并且其他线程都尝试获得这个锁时，那么它们将永远被阻塞。在线程A持有锁L并想获得锁M的同时，线程B持有锁M并尝试获得锁L，那么这两个线程将永远地等待下去。这种就是最简答的死锁形式（或者叫做"抱死"）。</div>


###### 解决办法
在Account中包含一个唯一的，不可变的值。比如说账号等。通过对这个值对对象进行排序。(snowFlakeId)
```
package com.example.demo.lock.method3;
import lombok.Data;
/**
 * @author songning
 */
@Data
public class Account {
    private Integer id;
    private Integer balance;
    public Account(Integer id, Integer balance) {
        this.id = id;
        this.balance = balance;
    }
    /**
     * 借记
     *
     * @param money
     * @throws Exception
     */
    public void debit(int money) throws Exception {
        Thread.sleep(500);
        balance = balance + money;
    }
    /**
     * 信贷
     *
     * @param money
     * @throws Exception
     */
    public void credit(int money) throws Exception {
        Thread.sleep(500);
        balance = balance - money;
    }
    public int compareTo(int money) {
        if (balance > money) {
            return 1;
        } else if (balance < money) {
            return -1;
        } else {
            return 0;
        }
    }
}
```

```
package com.example.demo.lock.method3;
/**
 * @author songning
 */
public class Helper {
    public void transfer(Account fromAccount, Account toAccount, int amount) throws Exception {
        if (fromAccount.compareTo(amount) < 0) {
            throw new Exception("金钱不够");
        } else {
            fromAccount.credit(amount);
            toAccount.debit(amount);
        }
    }
}
```

```
package com.example.demo.lock.method3;
import java.util.Arrays;
import java.util.List;
import java.util.Random;
/**
 * @author songning
 */
public class TransferThread extends Thread {
    public static List<Account> accountList = Arrays.asList(new Account(1, 1000), new Account(2, 1000));
    @Override
    public void run() {
        int i = new Random().nextInt(2);
        Account fromAccount, toAccount;
        if (i == 0) {
            fromAccount = accountList.get(1);
            toAccount = accountList.get(0);
        } else {
            fromAccount = accountList.get(0);
            toAccount = accountList.get(1);
        }
        try {
            Lock.transferMoney(fromAccount, toAccount, 1);
        } catch (Exception e) {
            System.out.println("发生异常-------" + e);
        }
    }
}
```

```
package com.example.demo.lock.method3;
/**
 * @author songning
 */
public class Lock {
    public static void transferMoney(Account fromAccount, Account toAccount, int amount) throws Exception {
        int fromId = fromAccount.getId();
        int toId = toAccount.getId();
        System.out.println("账户" + fromId + " 向账户 " + toId + " 转账");
        if (fromId < toId) {
            synchronized (fromAccount) {
                synchronized (toAccount) {
                    new Helper().transfer(fromAccount, toAccount, amount);
                }
            }
        } else if (fromId > toId) {
            synchronized (toAccount) {
                synchronized (fromAccount) {
                    new Helper().transfer(fromAccount, toAccount, amount);
                }
            }
        } else {
            throw new Exception("from,to不能相等!!!");
        }
    }
}
```

```
package com.example.demo.lock.method3;
/**
 * @author songning
 */
public class DemoMain {
    public static void main(String[] args) {
        for (int i = 0; i < 50; i++) {
            new TransferThread().start();
        }
    }
}

```



#### List<Person> personList筛选出满足age>20的集合? 
```
List<Person> result = personList.stream().filter((Person item) -> item.getAge() > 20).collect(Collectors.toList());

iterator遍历remove

倒序remove
```



#### 红黑树的5个特性
* 每个节点或者是黑色，或者是红色。
* 根节点是黑色。
* 每个叶子节点（NIL）是黑色。[注意：这里叶子节点，是指为空(NIL或NULL)的叶子节点！]
* 如果一个节点是红色的，则它的子节点必须是黑色的。
* 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点。[这里指到叶子节点的路径]



#### jvm常用命令参数
* jps
与unix上的ps类似，用来显示本地的java进程，可以查看本地运行着几个java程序，并显示他们的进程号。 


* jstat
一个极强的监视VM内存工具。可以用来监视VM内存内的各种堆和非堆的大小及其内存使用量。 
```
jstat -class pid: 显示加载class的数量，及所占空间等信息。 
jstat -compiler pid: 显示VM实时编译的数量等信息。 
jstat -gc pid: 可以显示gc的信息，查看gc的次数，及时间。其中最后五项，分别是young gc的次数，young gc的时间，full gc的次数，full gc的时间，gc的总时间。 
jstat -gccapacity: 可以显示，VM内存中三代（young,old,perm）对象的使用和占用大小，如：PGCMN显示的是最小perm的内存使用量，PGCMX显示的是perm的内存最大使用量，PGC是当前新生成的perm内存占用量，PC是但前perm内存占用量。其他的可以根据这个类推， OC是old内纯的占用量。 
jstat -gcnew pid: new对象的信息。 
jstat -gcnewcapacity pid: new对象的信息及其占用量。 
jstat -gcold pid: old对象的信息。 
jstat -gcoldcapacity pid: old对象的信息及其占用量。 
jstat -gcpermcapacity pid: perm对象的信息及其占用量。 
jstat -util pid: 统计gc信息统计。 
jstat -printcompilation pid: 当前VM执行的信息。 
除了以上一个参数外，还可以同时加上两个数字，如：jstat -printcompilation 3024 250 6是每250毫秒打印一次，一共打印6次，还可以加上-h3每三行显示一下标题。 
```


* **jmap**
一个可以输出所有内存中对象的工具，甚至可以将VM中的heap，以二进制输出成文本。 
```
jmap -dump:format=b,file=heap.bin <pid> 
file: 保存路径及文件名 
pid: 进程编号 
•jmap -histo:live  pid| less: 堆中活动的对象以及大小 
•jmap -heap pid: 查看堆的使用状况信息 
```


* jconsole
一个java GUI监视工具，可以以图表化的形式显示各种数据。并可通过远程连接监视远程的服务器VM。


* jstack
查看jvm线程运行状态，是否有死锁现象等等信息。
```
jstack pid: thread dump
```


* **jvisualvm**



#### 在整个项目开发过程中,你做过哪些优化?
* Redis缓存优化
<div style="text-indent:2em">在项目启动时implements ApplicationRunner实现public void run(ApplicationArguments args) {}方法，查询系统配置表和设备表，直接缓存到Redis中，在项目中可以直接查询Redis缓存而非数据库，提高了查询速度。</div>


* Mysql索引优化
<div style="text-indent:2em">项目中经常需要根据brand_code查询vehicle_brand表，因此brand_code添加索引。</div>

```
ALTER TABLE haiyan_vehicle_brand ADD INDEX brand_code_index(brand_code)
```


* Mysql限制返回条目数
<div style="text-indent:2em">在系统配置表里配置limit值，后台查询sql语句(Mysql或者ElasticSearch)的时候首先会添加limit约束。</div>

```
#if($limitSize)
LIMIT :limitSize
#end
```


* Mysql建表时使用固定长度字段和限制字段长度(e.g char(10))
  1. 降低物理存储空间
  2. 提高数据库处理速度
  3. 附带校验数据库是否合法功能
  
  
* 异步多线程操作取代同步单线程操作
<div style="text-indent:2em">某一业务场景下，第一次查询获取List<T>的结果集，然后根据T中的条件startTime,endTime,deviceIds再次查询,此时可以把第二次查询用异步操作执行并用Futrue接收，当所有的查询isDone()时，get()并return结果集。</div>


* 下载功能模块优化
<div style="text-indent:2em">很多页面需要实现下载的功能，包括图片和excel。走的是http请求，有时候因为数据量或者网络的问题导致请求响应超时。所以引进websocket来实现下载功能，整合所有的接口，并用线程中断技术interrupt实现下载取消的功能。</div>



#### 一次完整的HTTP请求过程
1. **DNS域名解析**
<div style="text-indent:2em">对请求的网址进行DNS域名解析，得到对应的IP地址。</div>


2. **建立TCP连接**
<div style="text-indent:2em">在HTTP工作开始之前，Web浏览器首先要通过网络与Web服务器建立连接，该连接是通过TCP来完成的，该协议与IP协议共同构建 Internet，即著名的TCP/IP协议，因此Internet又被称作是TCP/IP网络。HTTP是比TCP更高层次的应用层协议，根据规则，只有低层协议建立之后才能进行更高层协议的连接，因此，首先要建立TCP连接，一般TCP连接的端口号是80。建立TCP连接需要找到连接主机，所以需要先解析域名得到IP再找到主机进行3次握手建立TCP连接（两台电脑之间建立一个通信桥梁）</div>


3. **Web浏览器向Web服务器发送请求命令**
<div style="text-indent:2em">一旦建立了TCP连接，Web浏览器就会向Web服务器发送请求命令。例如：GET/hello/index.jsp HTTP/1.1。浏览器发送其请求命令之后，还要以头信息的形式向Web服务器发送一些别的信息(例：Accept ,User-Agent 等?)，之后浏览器发送了一空白行来通知服务器，它已经结束了该头信息的发送。</div>


4. **Web服务器应答**
<div style="text-indent:2em">客户机向服务器发出请求后，服务器会客户机进行应答，应答内容包括：协议的版本号和应答状态码：HTTP/1.1 200 OK，响应头信息来记录服务器自己的数据，被请求的文档内容。最后发送一个空白行来表示头信息的发送到此为结束，接着以Content-Type响应头信息所描述的格式发送用户所请求的实际数据。</div>


5. **Web服务器关闭TCP连接**
<div style="text-indent:2em">一般情况下，一旦 Web 服务器向浏览器发送了请求的数据，它就要关闭TCP连接，但是如果浏览器或者服务器在其头信息加入了这行代码：Connection:keep-alive。TCP连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求。保持连接节省了为每个请求建立新连接所需的时间，还节约了网络带宽。</div>


6. **浏览器接受到服务器响应的数据**
<div style="text-indent:2em">浏览器接受服务器应答回来的html代码和css，和js代码再进行页面的渲染或者接受到应答的文件进行保存等操作。</div>


###### 浏览器发起请求 -> 解析域名得到ip进行TCP连接 -> 浏览器发送HTTP请求和头信息发送 -> 服务器对浏览器进行应答，响应头信息和浏览器所需的内容 -> 关闭TCP连接或保持 -> 浏览器得到数据数据进行操作      



#### 除了synchronized关键字之外,你是怎么来保障线程安全的?
* Lock
* ThreadLocal
* AtomicInteger && AtomicReference



#### 常用的设计模式
* 策略模式

```
public interface StrategyService {
    void doSomething();
}

public class StrategyAImpl implements StrategyService {
    @Override
    public void doSomething() {
        System.out.println("策略A的实现");
    }
}

public class StrategyBImpl implements StrategyService {
    @Override
    public void doSomething() {
        System.out.println("策略B的实现");
    }
}

public class Context {
    private StrategyService strategyService;
    public Context(StrategyService strategyService) {
        this.strategyService = strategyService;
    }
    public void executeStrategy() {
        this.strategyService.doSomething();
    }
}

public class Client {
    public static void main(String[] args) {
        StrategyService strategyA = new StrategyAImpl();  
        Context contextA = new Context(strategyA);
        contextA.executeStrategy();
        StrategyService strategyB = new StrategyBImpl();
        Context contextB = new Context(strategyB);
        contextB.executeStrategy();
    }
}
```


* 工厂模式

```
public interface Shape {
   void draw();
}

public class Rectangle implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}

public class Square implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside Square::draw() method.");
   }
}

public class Circle implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside Circle::draw() method.");
   }
}

public class ShapeFactory {
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }        
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      return null;
   }
}

public class FactoryPatternDemo {
   public static void main(String[] args) {
      ShapeFactory shapeFactory = new ShapeFactory();
      //获取 Circle 的对象，并调用它的 draw 方法
      Shape shape1 = shapeFactory.getShape("CIRCLE");
      //调用 Circle 的 draw 方法
      shape1.draw();
      //获取 Rectangle 的对象，并调用它的 draw 方法
      Shape shape2 = shapeFactory.getShape("RECTANGLE");
      //调用 Rectangle 的 draw 方法
      shape2.draw();
      //获取 Square 的对象，并调用它的 draw 方法
      Shape shape3 = shapeFactory.getShape("SQUARE");
      //调用 Square 的 draw 方法
      shape3.draw();
   }
}
```


* 单例模式

```
// 懒汉模式，线程不安全
public class Singleton {  
    private static Singleton instance;   
    public static Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}

// 懒汉模式，线程安全
public class Singleton {  
    private static Singleton instance;  
    public static synchronized Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}

// 饿汉模式，线程安全
public class Singleton {  
    private static Singleton instance = new Singleton();  
    public static Singleton getInstance() {  
        return instance;  
    }  
}

// 双检锁/双重校验锁，线程安全
public class Singleton {  
    private volatile static Singleton singleton;  
    public static Singleton getSingleton() {  
        if (singleton == null) {  
            synchronized (Singleton.class) {  
                if (singleton == null) {  
                    singleton = new Singleton();  
                }  
            }  
        }  
        return singleton;  
    }  
}
```


* 代理模式

```
// 静态代理

public interface PrinterService {
    void print();
}
public class PrinterServiceImpl implements PrinterService {
    public void print(){
        System.out.println("打印！");
    }
}
public class PrinterProxy implements PrinterService {
    private PrinterService printerService;
    public PrinterProxy(){
        this.printerService = new PrinterServiceImpl();
    }
    @Override
    public void print() {
        System.out.println("记录日志");
        printerService.print();
    }
}
public class Test {
    public static void main(String[] args) {
        PrinterProxy proxy = new PrinterProxy();
        proxy.print();
    }
}


// 动态代理

public class ProxyHandler implements InvocationHandler {
    private Object targetObject;//被代理的对象
    //将被代理的对象传入获得它的类加载器和实现接口作为Proxy.newProxyInstance方法的参数。
    public  Object newProxyInstance(Object targetObject){
        this.targetObject = targetObject;
        //targetObject.getClass().getClassLoader()：被代理对象的类加载器
        //targetObject.getClass().getInterfaces()：被代理对象的实现接口
        //this：当前对象，该对象实现了InvocationHandler接口所以有invoke方法，通过invoke方法可以调用被代理对象的方法
        return Proxy.newProxyInstance(targetObject.getClass().getClassLoader(),targetObject.getClass().getInterfaces(),this);
    }
    //该方法在代理对象调用方法时调用
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("记录日志");
        return method.invoke(targetObject,args);
    }
}
public class Test {
    public static void main(String[] args){
        ProxyHandler proxyHandler=new ProxyHandler();
        PrinterServiceImpl printerServiceImpl = (PrinterService) proxyHandler.newProxyInstance(new PrinterServiceImpl());
        printerServiceImpl.print();
    }
}

```



#### 常用的负载均衡策略
* 轮询(默认)
每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
```
upstream backserver {
    server 192.168.0.2;
    server 192.168.0.3;
}
```


* 指定权重
指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。
```
upstream backserver {
    server 192.168.0.14 weight=4;
    server 192.168.0.15 weight=6;
}
```


* IP绑定ip_hash
上述的两个方式存在一个问题，就是说在负载均衡系统中，如果用户在某台服务器上登录了，那么该用户第二次请求的时候，因为我们是负载均衡系统，每次请求都会重新定位到服务器集群中的某一个，那么已经登录某一个服务器的用户再重新定位到另一个服务器，其登录信息将会丢失，这样显然是不妥的。我们可以采用ip_hash指令解决这个问题，如果客户已经访问了某个服务器，当用户再次访问时，会将该请求通过哈希算法，自动定位到该服务器。每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
```
upstream backserver {
    ip_hash;
    server 192.168.0.2:88;
    server 192.168.0.3:80;
}
```


* fair(第三方)
按后端服务器的响应时间来分配请求，响应时间短的优先分配。
```
upstream backserver {
    server server1;
    server server2;
    fair;
}
```


* url_hash(第三方)
按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。
```
upstream backserver {
    server squid1:3128;
    server squid2:3128;
    hash $request_uri;
    hash_method crc32;
}
```



#### ES读写搜索数据的过程?

###### ES写数据
1. 客户端选择一个node发送请求过去，这个node就是coordinating node(协调节点)。
2. coordinating node对document进行路由，将请求转发给对应的node(有primary shard)。
3. 实际的node上的primary shard处理请求，然后将数据同步到replica node。
4. coordinating node如果发现primary node和所有replica node都搞定之后，就返回响应结果给客户端。

###### ES读数据
1. 客户端发送请求到任意一个node，成为coordinate node。
2. coordinate node对doc id进行哈希路由，将请求转发到对应的node，此时会使用round-robin随机轮询算法，在primary shard以及其所有replica中随机选择一个，让读请求负载均衡。
3. 接收请求的node返回document给coordinate node。
4. coordinate node返回document给客户端。

###### ES搜索数据
1. 客户端发送请求到一个coordinate node。
2. 协调节点将搜索请求转发到所有的shard对应的primary shard或replica shard，都可以。
3. query phase: 每个shard将自己的搜索结果(其实就是一些doc id)返回给协调节点，由协调节点进行数据的合并、排序、分页等操作，产出最终结果。
4. fetch phase: 接着由协调节点根据doc id去各个节点上拉取实际的document数据，最终返回给客户端。



#### ES倒排索引?
关键词到文档ID的映射，每个关键词都对应着一系列的文件，这些文件中都出现了关键词。


#### TCP协议?
3次握手，4次挥手。



#### Spring AOP?
* 定义
<div style="text-indent:2em">面向切面编程，在程序开发中主要用来解决一些系统层面上的问题，比如日志，事务，权限。</div>
* 通知类型介绍
  1. Before: 在目标方法被调用之前做增强处理,@Before只需要指定切入点表达式即可。
  2. AfterReturning: 在目标方法正常完成后做增强,@AfterReturning除了指定切入点表达式后，还可以指定一个返回值形参名returning,代表目标方法的返回值。
  3. AfterThrowing: 主要用来处理程序中未处理的异常,@AfterThrowing除了指定切入点表达式后，还可以指定一个throwing的返回值形参名,可以通过该形参名来访问目标方法中所抛出的异常对象。
  4. After: 在目标方法完成之后做增强，无论目标方法时候成功完成。@After可以指定一个切入点表达式。
  5. Around: 环绕通知,在目标方法完成前后做增强处理,环绕通知是最重要的通知类型,像事务,日志等都是环绕通知,注意编程中核心是一个ProceedingJoinPoint。
* 使用场景
  1. Authentication: 权限
  2. Caching: 缓存
  3. Context passing: 内容传递
  4. Error handling: 错误处理
  5. Lazy loading: 懒加载
  6. Debugging: 调试
  7. logging tracing profiling monitoring: 记录 跟踪 优化 校准
  8. Performance optimization: 性能优化
  9. Persistence: 持久化
  10. Resource pooling: 资源池
  11. Synchronization: 同步
  12. Transactions: 事务



#### 如何自定义一个注解?

1. 创建 @ Annotation 类
2. @Target: 指定该注解的作用目标
```
@Target(ElementType.TYPE)   // 接口 类 枚举 注解
@Target(ElementType.FIELD)  // 字段 枚举的常量
@Target(ElementType.METHOD) // 方法
@Target(ElementType.PARAMETER)  // 方法参数
@Target(ElementType.CONSTRUCTOR)    //构造函数
@Target(ElementType.LOCAL_VARIABLE) // 局部变量
@Target(ElementType.ANNOTATION_TYPE)    // 注解
@Target(ElementType.PACKAGE)    // 包
```
3. @Retention: 指定该注解的保留策略
```
@Retention(RetentionPolicy.SOURCE)  // 注解仅存在于源码中，在class字节码文件中不包含
@Retention(RetentionPolicy.CLASS)   // 默认的保存策略，注解会在class字节码文件中存在，但运行时无法获得
@Retention(RetentionPolicy.RUNTIME) // 注解会在class字节码文件中存在，在运行时可以通过反射获取到
```
4. @Inherited: 指定该注解是否可以被继承



#### HTTPS请求流程?
![](/images/ActualCombat/1266003527524679725.png)


1. 客户端向服务器发起HTTPS请求，连接到服务器的443端口。
2. 服务器端有一个密钥对，即公钥和私钥，是用来进行非对称加密使用的，服务器端保存着私钥，不能将其泄露，公钥可以发送给任何人。
3. 服务器将自己的公钥发送给客户端。
4. 客户端收到服务器端的证书之后，会对证书进行检查，验证其合法性，如果发现发现证书有问题，那么HTTPS传输就无法继续。严格的说，这里应该是验证服务器发送的数字证书的合法性，关于客户端如何验证数字证书的合法性，下文会进行说明。如果公钥合格，那么客户端会生成一个随机值，这个随机值就是用于进行对称加密的密钥，我们将该密钥称之为client key，即客户端密钥，这样在概念上和服务器端的密钥容易进行区分。然后用服务器的公钥对客户端密钥进行非对称加密，这样客户端密钥就变成密文了，至此，HTTPS中的第一次HTTP请求结束。
5. 客户端会发起HTTPS中的第二个HTTP请求，将加密之后的客户端密钥发送给服务器。
6. 服务器接收到客户端发来的密文之后，会用自己的私钥对其进行非对称解密，解密之后的明文就是客户端密钥，然后用客户端密钥对数据进行对称加密，这样数据就变成了密文。
7. 然后服务器将加密后的密文发送给客户端。
8. 客户端收到服务器发送来的密文，用客户端密钥对其进行对称解密，得到服务器发送的数据。这样HTTPS中的第二个HTTP请求结束，整个HTTPS传输完成。



#### @SpringBootApplication包含哪些注解?

* @ComponentScan
<div style="text-indent:2em">扫描当前包及其子包下被@Component，@Controller，@Service，@Repository注解标记的类并纳入到Spring容器进行管理。</div>


* @SpringBootConfiguration
<div style="text-indent:2em">继承自@Configuration，二者功能一致，标注当前类是配置类，并会将当前类内声明的一个或多个以@Bean注解标记的方法的实例纳入到Spring容器中，并且实例名就是方法名。</div>


* EnableAutoConfiguration
<div style="text-indent:2em">启动自动的配置，@EnableAutoConfiguration注解的意思就是SpringBoot根据你添加的jar包来配置你项目的默认配置。</div>




#### JVM参数调优?

###### 堆设置
* -Xms: 初始堆大小。
* -Xmx: 最大堆大小。
* -XX:NewSize=n: 设置年轻代大小。
* -XX:NewRatio=n: 设置年轻代和年老代的比值。如:为3，表示年轻代与年老代比值为1:3，年轻代占整个年轻代年老代和的1/4。
* -XX:SurvivorRatio=n: 年轻代中Eden区与两个Survivor区的比值。注意Survivor区有两个。如:3，表示Eden:Survivor=3:2，一个Survivor区占整个年轻代的1/5 。
* -XX:MaxPermSize=n: 设置持久代大小。

###### 收集器设置
* -XX:+UseSerialGC: 设置串行收集器。
* -XX:+UseParallelGC: 设置并行收集器。
* -XX:+UseParalledlOldGC: 设置并行年老代收集器。
* -XX:+UseConcMarkSweepGC: 设置并发收集器。


#### JVM常用命令?
* jps
* **jmap -heap pid**: 获取垃圾回收器的类型和系统参数
* jinfo -flags pid: 查看应用启动的参数
* **jstack -l pid**: 查看线程的运行信息(包括死锁的线程)
* jstat -gcutil pid: 统计gc信息
* jconsole
* jvisualvm



#### mysql有哪些索引并说说他们的区别?

###### Hash索引 && B+树索引
* 如果是等值查询，那么哈希索引明显有绝对优势，因为只需要经过一次算法即可找到相应的键值；当然了，这个前提是，键值都是唯一的。如果键值不是唯一的，就需要先找到该键所在位置，然后再根据链表往后扫描，直到找到相应的数据。
* 从示意图中也能看到，如果是范围查询检索，这时候哈希索引就毫无用武之地了，因为原先是有序的键值，经过哈希算法后，有可能变成不连续的了，就没办法再利用索引完成范围查询检索。
* 同理，哈希索引也没办法利用索引完成排序，以及like 'xxx%' 这样的部分模糊查询（这种部分模糊查询，其实本质上也是范围查询）。
* 哈希索引也不支持多列联合索引的最左匹配规则。
* B+树索引的关键字检索效率比较平均，不像B树那样波动幅度大，在有大量重复键值情况下，哈希索引的效率也是极低的，因为存在所谓的哈希碰撞问题。



#### SQL语句性能的优化?
* 对查询进行优化，应尽量避免全表扫描，首先应考虑在where和order by涉及的列上建立索引。
* 注意前面罗列的会使索引失效的那些运算符，这样SQL是无法使用索引的。
  1. like后面的通配符在前面，索引会失效。
  2. 没有使用联合索引的第一列，not in，!=，使用MySQL函数，类型转换，or等都无法用到索引。
* 应尽量避免在where子句中使用or来连接条件，否则将导致MySQL放弃使用索引而进行全表扫描，如：select id from t where num=10 or num=20可以这样查询：select id from t where num=10 union all select id from t where num=20。
* in和not in也要慎用，否则会导致全表扫描，如：select id from t where num in(1,2,3); 对于连续的数值，能用between就不要用in了：select id from t where num between 1 and 3。
* 应尽量避免在where子句中对字段进行表达式操作，这将导致引擎放弃使用索引而进行全表扫描。如：select id from t where num/2=100应改为:select id from t where num=100*2。
* 应尽量避免在where子句中对字段进行函数操作，这将导致引擎放弃使用索引而进行全表扫描。如：select id from t where substring(name,1,3)=‘abc’ ，name以abc开头的id，应改为:select id from t where name like 'abc%'。
* 不要在where子句中的“=”左边进行函数、算术运算或其他表达式运算，否则系统将可能无法正确使用索引。
* 在使用索引字段作为条件时，如果该索引是复合索引，那么必须使用到该索引中的第一个字段作为条件时才能保证系统使用该索引，否则该索引将不会被使用，并且应尽可能的让字段顺序与索引顺序相一致。
* 并不是所有索引对查询都有效，SQL是根据表中数据来进行查询优化的，当索引列有大量数据重复时，SQL查询可能不会去利用索引，如一表中有字段sex，male、female几乎各一半，那么即使在sex上建了索引也对查询效率起不了作用，这个是属于MySQL的SQL优化器对索引的一种优化。
* 索引并不是越多越好，索引固然可以提高相应的select的效率，但同时也降低了insert及update的效率，因为insert或update时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有必要。
* 尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这会降低查询和连接的性能，并会增加存储开销。这是因为引擎在处理查询和连接时会逐个比较字符串中每一个字符，而对于数字型而言只需要比较一次就够了。
* 尽可能的使用varchar/nvarchar代替char/nchar，因为首先变长字段存储空间小，可以节省存储空间，其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些。
  


#### 查找指定文件
`find  path expression`



#### 你知道top命令吗?
* 定义
<div style="text-indent:2em">top命令是Linux下常用的性能分析工具，能够实时显示系统中各个进程的资源占用状况，类似于Windows的任务管理器。top显示系统当前的进程和其他状况,是一个动态显示过程,即可以通过用户按键来不断刷新当前状态。如果在前台执行该命令,它将独占前台,直到用户终止该程序为止。比较准确的说,top命令提供了实时的对系统处理器的状态监视。它将显示系统中CPU最“敏感”的任务列表。该命令可以按CPU使用。内存使用和执行时间对任务进行排序；而且该命令的很多特性都可以通过交互式命令或者在个人定制文件中进行设定。</div>
* 常用操作
  1. top: 每隔5秒显式所有进程的资源占用情况。
  2. top -d 2: 每隔2秒显式所有进程的资源占用情况。
  3. top -c: 每隔5秒显式进程的资源占用情况，并显示进程的命令行参数(默认只有进程名)。
  4. top -p 12345 -p 6789: 每隔5秒显示pid是12345和pid是6789的两个进程的资源占用情况。
  5. top -d 2 -c -p 123456: 每隔2秒显示pid是12345的进程的资源使用情况，并显式该进程启动的命令行参数。