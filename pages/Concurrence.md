#### 同步方法和同步代码块的区别是什么？
在Java语言中，每一个对象有一把锁。线程可以使用synchronized关键字来获取对象上的锁。synchronized关键字可应用在方法级别(粗粒度锁)或者是代码块级别(细粒度锁)。


#### 在监视器(Monitor)内部，是如何做线程同步的？程序应该做哪种级别的同步？
监视器和锁在Java虚拟机中是一块使用的。监视器监视一块同步代码块，确保一次只有一个线程执行同步代码块。每一个监视器都和一个对象引用相关联，线程在获取锁之前不允许执行同步代码。


#### 什么是死锁(deadlock)？如何才能产生死锁？
两个进程都在等待对方执行完毕才能继续往下执行的时候就发生了死锁，结果就是两个进程都陷入了无限的等待中。


| 条件 | 解释 | 
| :----- | :----- | 
| 互斥条件 | 所谓互斥就是进程在某一时间内独占资源 | 
| 请求与保持条件 | 一个进程因请求资源而阻塞时，对已获得的资源保持不放 | 
| 不剥夺条件 | 进程已获得资源，在末使用完之前，不能强行剥夺 | 
| 循环等待条件 | 若干进程之间形成一种头尾相接的循环等待资源关系 | 


#### 如何预防死锁？
| 条件 | 解释 | 
| :----- | :----- | 
| 打破互斥条件 | <div style="width: 550px">即允许进程同时访问某些资源。但是，有的资源是不允许被同时访问的，像打印机等等，这是由资源本身的属性所决定的。所以，这种办法并无实用价值</div> | 
| 打破不可抢占条件 | <div style="width: 550px">即允许进程强行从占有者那里夺取某些资源。就是说，当一个进程已占有了某些资源，它又申请新的资源，但不能立即被满足时，它必须释放所占有的全部资源，以后再重新申请。它所释放的资源可以分配给其它进程。这就相当于该进程占有的资源被隐蔽地强占了。这种预防死锁的方法实现起来困难，会降低系统性能</div> | 
| 打破占有且申请条件 | <div style="width: 550px">可以实行资源预先分配策略。即进程在运行前一次性地向系统申请它所需要的全部资源。如果某个进程所需的全部资源得不到满足，则不分配任何资源，此进程暂不运行。只有当系统能够满足当前进程的全部资源需求时，才一次性地将所申请的资源全部分配给该进程。由于运行的进程已占有了它所需的全部资源，所以不会发生占有资源又申请资源的现象，因此不会发生死锁</div> | 
| 打破循环等待条件 | <div style="width: 550px">实行资源有序分配策略。采用这种策略，即把资源事先分类编号，按号分配，使进程在申请，占用资源时不会形成环路。所有进程对资源的请求必须严格按资源序号递增的顺序提出。进程占用了小号资源，才能申请大号资源，就不会产生环路，从而预防了死锁</div> | 


#### 如何确保N个线程可以访问N个资源同时又不导致死锁？
使用多线程的时候，一种非常简单的避免死锁的方式就是：指定获取锁的顺序，并强制线程按照指定的顺序获取锁。因此，如果所有的线程都是以同样的顺序加锁和释放锁，就不会出现死锁了。


#### <a href="https://www.jianshu.com/p/570410236ff5">说说synchronized关键字的底层原理？</a>
synchronized同步语句块的实现，使用的是**monitorenter**和**monitorexit**指令，其中monitorenter指令指向同步代码块的开始位置，monitorexit指令则指明同步代码块的结束位置。 当执行monitorenter指令时，线程试图获取锁，也就是获取monitor(monitor对象存在于每个Java对象的对象头中，synchronized锁便是通过这种方式获取锁的，这也是为什么Java中任意对象都可以作为锁的原因)的持有权。当计数器为0，则可以成功获取，获取后将锁计数器设为1，也就是加1；相应的，在执行monitorexit指令后，将锁计数器设为0，表明锁被释放。如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。


#### synchronized如何使用？
| 分类 | 被锁的对象 | 伪代码 | 解释 | 
| :----- | :----- | :----- | :----- | 
| <div style="width: 100px">实例方法</div> | <div style="width: 160px">类的实例对象</div> | public synchronized void method() { ... } | 实例方法，锁住的是该类的实例对象 |
| <div style="width: 100px">静态方法</div> | <div style="width: 160px">类对象</div> | public static synchronized void method() { ... } | 静态方法，锁住的是类对象 |
| <div style="width: 100px">实例对象</div> | <div style="width: 160px">类的实例对象</div> | synchronized(this) { ... } | 同步代码块，锁住的是该类的实例对象 |
| <div style="width: 100px">Class对象</div> | <div style="width: 160px">类对象</div> | synchronized(Clazz.class) { ... } | 同步代码块，锁住的是该类的类对象 |
| <div style="width: 100px">任意实例对象</div> | <div style="width: 160px">实例对象Object</div> | Object object = new Object(); synchronized(object) { ... } | 同步代码块，锁住的是配置的实例对象 |


#### synchronized和Lock的区别？
| synchronized | Lock | 
| :----- | :----- | 
| 关键字，内置语言实现 | Lock是接口 | 
| 在线程发生异常时会自动释放锁，因此不会发生异常死锁 | 异常时不会自动释放锁，所以需要在finally中实现释放锁 | 
| 非中断锁，必须等待线程执行完成释放锁 | 中断锁 | 
| ———— | 使用读锁提高多线程读效率 | 


#### 同步关键字(synchronized)修饰静态方法和修饰非静态方法的区别？
* **修饰非静态方法**


```
class Demo {
    private int i;
    Demo(int i) {
        this.i = i;
    }
    public synchronized void getI(String s) {
        for (int j = 0; j < 10000; j++) {
            System.out.println(i + "---------" + s);
        }
    }
}

public class ObjectLock implements Runnable {
    Demo demo;
    public ObjectLock(Demo demo) {
        this.demo = demo;
    }
    @Override
    public void run() {
        demo.getI(Thread.currentThread().getName());
    }
    public static void main(String[] args) {
        Demo demo = new Demo(1);
        Thread thread = new Thread(new ObjectLock(demo), "thread0");
        Thread thread1 = new Thread(new ObjectLock(demo), "thread1");
        Thread thread2 = new Thread(new ObjectLock(new Demo(2)), "thread2");
        thread.start();
        thread1.start();
        thread2.start();
    }
}
```

```
// 非静态方法结果
...
1---------thread0
1---------thread0
2---------thread2
1---------thread0
2---------thread2
1---------thread0
1---------thread1
1---------thread1
2---------thread2
1---------thread1
1---------thread1
...
```
<span style="color: red">可以看到Thread0和Thread2交替出现，Thread1和Thread2交替出现，但Thread0和Thread1不会交替出现。因为对非静态方法加锁，实际上是对调用该方法的对象加锁。Thread0和Thread1用的是同一个对象，所以互斥，但是Thread2则不受影响。</span>


* **修饰静态方法**


```
class Demo {
    static int i;
    Demo(int i) {
        Demo.i = i;
    }
    public static synchronized void staticGetI(String s) {
        for (int j = 0; j < 10000; j++) {
            System.out.println(i + "---------" + s);
        }
    }
}

// ObjectLock重复部分略
...
    @Override
    public void run() {
        Demo.staticGetI(Thread.currentThread().getName());
    }
...
```


```
// 静态方法结果
...
2---------thread0
2---------thread0
2---------thread2
2---------thread2
2---------thread1
2---------thread1
...
```
<span style="color: red">三个线程均互斥。当synchronized修饰一个static方法时，多线程下，获取的是类锁(即Class本身，注意：不是实例)，作用范围是整个静态方法，作用的对象是这个类的所有对象。</span>


* **一个对象在两个线程中分别调用一个静态同步方法和一个非静态同步方法**


```
...
public void run() {
    if (Thread.currentThread().getName().equals("thread0")){
        demo.staticGetI(Thread.currentThread().getName());
    }else if (Thread.currentThread().getName().equals("thread1")){
        demo.getI(Thread.currentThread().getName());
    }
} 
...
```
<span style="color: red">不会产生互斥。因为虽然是一个对象调用，但是两个方法的锁类型不同，调用的静态方法实际上是类对象在调用，即这两个方法产生的并不是同一个对象锁，因此不会互斥，会并发执行。</span>


#### 能聊聊你对CAS的理解以及它的底层原理吗？
**<span style="color: red">内存值V，预期值A，要更新的新值B</span>**


![CAS](/images/Concurrence/CAS.png)


t1和t2线程都同时去访问同一变量V=56，所以他们会把主内存的值V完全拷贝一份到自己的工作内存空间，所以t1和t2线程的预期值都为V=56。假设t1在与t2线程竞争中线程t1能去更新变量的值，而其他线程都失败(失败的线程并不会被挂起，而是被告知这次竞争中失败，并可以再次发起尝试)。t1线程去更新变量值改为B=57，然后写到内存中。此时对于t2来说，内存值变为了57，与预期值56不一致，就操作失败了(想改的值不再是原来的值)。


就是指当两者进行比较时，如果相等，则证明共享数据没有被修改，替换成新值，然后继续往下运行；如果不相等，说明共享数据已经被修改，放弃已经所做的操作，然后重新执行刚才的操作。容易看出CAS操作是基于共享数据不会被修改的假设，采用了类似于数据库的commit-retry的模式。当同步冲突出现的机会很少时，这种假设能带来较大的性能提升。


#### <a href="https://segmentfault.com/a/1190000017372067">你对JDK的AQS理解吗？AQS的实现原理是什么？</a>
![AQS](/images/Concurrence/AQS.jpg)


#### <a href="https://www.jianshu.com/p/15106e9c4bf3">谈谈你对Java内存模型的理解？</a>
![JMM](/images/Concurrence/JMM.png)



