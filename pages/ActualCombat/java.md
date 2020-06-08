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
