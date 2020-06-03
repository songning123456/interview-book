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
这两种情况锁的都是类的实例对象，如果Test testObj1=new Test();现在两个线程thread1,thread2，如果thread1访问Test.test1或Test.test2那么thread2是无法同时访问的，因为Test对象的锁已经被thread1使用了，只有thread1释放锁之后才能被thread2占用；如果再有一个对象Test testObj2=new test();此时thread1访问testObj1.testMethod1方法，thread2访问testObj2.testMethod1方法是互不影响的。


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
银行家算法


#### List<Person> personList筛选出满足age>20的集合? 
```
List<Person> result = personList.stream().filter((Person item) -> item.getAge() > 20).collect(Collectors.toList());
```