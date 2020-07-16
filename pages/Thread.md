#### 进程和线程的区别是什么？
进程是执行着的应用程序，而线程是进程内部的一个执行序列。一个进程可以有多个线程。线程又叫做轻量级进程。


#### 创建线程有几种不同的方式？你喜欢哪一种？为什么？
有三种方式可以用来创建线程。


继承Thread类，实现Runnable接口，应用程序可以使用Executor框架来创建线程池。


实现Runnable接口这种方式更受欢迎，因为这不需要继承Thread类。在应用设计中已经继承了别的对象的情况下，这需要多继承(而Java不支持多继承)，只能实现接口。同时，线程池也是非常高效的，很容易实现和使用。


#### 概括的解释下线程的几种可用状态？
| 状态 | 解释 | 
| :---- | :---- |
| 就绪(Runnable) | 线程准备运行，不一定立马就能开始执行 |
| 运行中(Running) | 进程正在执行线程的代码 |
| 等待中(Waiting) | 线程处于阻塞的状态，等待外部的处理结束 |
| 睡眠中(Sleeping) | 线程被强制睡眠 |
| I/O阻塞(Blocked on I/O) | 等待I/O操作完成 |
| 同步阻塞(Blocked on Synchronization) | 等待获取锁 |
| 死亡(Dead) | 线程完成了执行 |


#### 什么是线程安全？如何保证线程安全?
线程安全就是多线程访问同一代码，不会产生不确定的结果。


对非安全的代码进行加锁控制；使用线程安全的类；多线程并发情况下，线程共享的变量改为方法级的局部变量。


#### 怎么停止一个线程？
使用退出标志，使线程正常退出，也就是当run方法完成后线程终止。


使用stop方法强行终止，但是不推荐这个方法，因为stop和suspend及resume一样都是过期作废的方法。


使用interrupt方法中断线程。


#### sleep和wait的区别？
| sleep | wait | 
| :----- | :----- | 
| Thread类中方法 | Object类中的方法 | 
| 可以在任何地方使用 | 只能在同步代码块或同步方法中使用 | 
| 程序暂停执行指定的时间，让出cpu该其他线程，但是他的监控状态依然保持者，当指定的时间到了又会自动恢复运行状态，在调用sleep()方法的过程中，线程不会释放对象锁 | 线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用notify()方法后本线程才进入对象锁定池准备 | 


#### 多线程之间是如何进行信息交互的？
| 方法 | 解释 | 
| :----- | :----- | 
| void notify() | 唤醒在此对象监视器上等待的单个线程 | 
| void notifyAll() | 唤醒在此对象监视器上等待的所有线程 | 
| void wait() | 导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法 | 
| void wait(long timeout) | 导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法，或者超过指定的时间量 | 
| void wait(long timeout,int nanos) | 导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量 | 


#### 多线程的几种通讯方式？
* **同步**——多个线程通过synchronized关键字这种方式来实现线程间的通信。


```
public class MyObject {
    synchronized public void methodA() {
        //do something....
    }
    synchronized public void methodB() {
        //do some other thing
    }
}

public class ThreadA extends Thread {
    private MyObject object;
    @Override
    public void run() {
        super.run();
        object.methodA();
    }
}

public class ThreadB extends Thread {
    private MyObject object;
    @Override
    public void run() {
        super.run();
        object.methodB();
    }
}

public class Run {
    public static void main(String[] args) {
        MyObject object = new MyObject();
        //线程A与线程B 持有的是同一个对象:object
        ThreadA a = new ThreadA(object);
        ThreadB b = new ThreadB(object);
        a.start();
        b.start();
    }
}
```
由于线程A和线程B持有同一个MyObject类的对象object，尽管这两个线程需要调用不同的方法，但是它们是同步执行的，比如：线程B需要等待线程A执行完了methodA()方法之后，它才能执行methodB()方法。这样，线程A和线程B就实现了通信。这种方式，本质上就是“共享内存”式的通信。多个线程需要访问同一个共享变量，谁拿到了锁(获得了访问权限)，谁就可以执行。


* **while轮询的方式**


```
import java.util.ArrayList;
import java.util.List;
public class MyList {
    private List<String> list = new ArrayList<String>();
    public void add() {
        list.add("elements");
    }
    public int size() {
        return list.size();
    }
}
 
import mylist.MyList;
public class ThreadA extends Thread {
    private MyList list;
    public ThreadA(MyList list) {
        super();
        this.list = list;
    }
    @Override
    public void run() {
        try {
            for (int i = 0; i < 10; i++) {
            list.add();
            System.out.println("添加了" + (i + 1) + "个元素");
            Thread.sleep(1000);
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
 
import mylist.MyList;
public class ThreadB extends Thread {
    private MyList list;
    public ThreadB(MyList list) {
        super();
        this.list = list;
    }
    @Override
    public void run() {
        try {
            while (true) {
                if (list.size() == 5) {
                    System.out.println("==5, 线程b准备退出了");
                    throw new InterruptedException();
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

import mylist.MyList;
import extthread.ThreadA;
import extthread.ThreadB;
public class Test {
    public static void main(String[] args) {
        MyList service = new MyList();
        ThreadA a = new ThreadA(service);
        a.setName("A");
        a.start();
        ThreadB b = new ThreadB(service);
        b.setName("B");
        b.start();
    }
}
```
在这种方式下，线程A不断地改变条件，线程ThreadB不停地通过while语句检测这个条件(list.size()==5)是否成立 ，从而实现了线程间的通信。但是这种方式会浪费CPU资源。之所以说它浪费资源，是因为JVM调度器将CPU交给线程B执行时，它没做啥“有用”的工作，只是在不断地测试某个条件是否成立。就类似于现实生活中，某个人一直看着手机屏幕是否有电话来了，而不是在干别的事情，当有电话来时，响铃通知TA电话来了。这种方式还存在另外一个问题：轮询的条件的可见性问题。线程都是先把变量读取到本地线程栈空间，然后再去再去修改的本地变量。因此，如果线程B每次都在取本地的条件变量，那么尽管另外一个线程已经改变了轮询的条件，它也察觉不到，这样也会造成死循环。


* **wait/notify机制**


```
import java.util.ArrayList;
import java.util.List;
public class MyList {
    private static List<String> list = new ArrayList<String>();
    public static void add() {
        list.add("anyString");
    }
    public static int size() {
        return list.size();
    }
}

public class ThreadA extends Thread {
    private Object lock;
    public ThreadA(Object lock) {
        super();
        this.lock = lock;
    }
    @Override
    public void run() {
        try {
            synchronized (lock) {
                if (MyList.size() != 5) {
                    System.out.println("wait begin " + System.currentTimeMillis());
                    lock.wait();
                    System.out.println("wait end " + System.currentTimeMillis());
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class ThreadB extends Thread {
    private Object lock;
    public ThreadB(Object lock) {
        super();
        this.lock = lock;
    }
    @Override
    public void run() {
        try {
            synchronized (lock) {
                for (int i = 0; i < 10; i++) {
                    MyList.add();
                    if (MyList.size() == 5) {
                        lock.notify();
                        System.out.println("已经发出了通知");
                    }
                    System.out.println("添加了" + (i + 1) + "个元素!");
                    Thread.sleep(1000);
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class Run {
    public static void main(String[] args) {
        try {
            Object lock = new Object();
            ThreadA a = new ThreadA(lock);
            a.start();
            Thread.sleep(50);
            ThreadB b = new ThreadB(lock);
            b.start();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
线程A要等待某个条件满足时(list.size()==5)，才执行操作。线程B则向list中添加元素，改变list的size。A,B之间如何通信的呢？也就是说，线程A如何知道list.size()已经为5了呢？这里用到了Object类的wait()和notify()方法。当条件未满足时(list.size()!=5)，线程A调用wait()放弃CPU，并进入阻塞状态。不像while轮询那样占用CPU。当条件满足时，线程B调用notify()通知线程A，所谓通知线程A，就是唤醒线程A，并让它进入可运行状态。这种方式的一个好处就是CPU的利用率提高了。但是也有一些缺点：比如，线程B先执行，一下子添加了5个元素并调用了notify()发送了通知，而此时线程A还执行；当线程A执行并调用wait()时，那它永远就不可能被唤醒了。因为，线程B已经发了通知了，以后不再发通知了。这说明：通知过早，会打乱程序的执行逻辑。


* **管道通信**——java.io.PipedInputStream 和 java.io.PipedOutputStream


#### 说说线程池的创建方式？
**newCachedThreadPool**——创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。


```
// 无限大小线程池JVM自动回收
ExecutorService newCachedThreadPool = Executors.newCachedThreadPool();
for (int i = 0; i < 10; i++) {
    final int temp = i;
    newCachedThreadPool.execute(new Runnable() {
        @Override
        public void run() {
            try {
                Thread.sleep(100);
            } catch (Exception e) {
                // TODO: handle exception
            }
            System.out.println(Thread.currentThread().getName() + ",i:" + temp);
        }
    });
}
```
<span style="color: red">总结：线程池为无限大，当执行第二个任务时第一个任务已经完成，会复用执行第一个任务的线程，而不用每次新建线程。</span>


**newFixedThreadPool**——创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
```
ExecutorService newFixedThreadPool = Executors.newFixedThreadPool(5);
for (int i = 0; i < 10; i++) {
    final int temp = i;
    newFixedThreadPool.execute(new Runnable() {
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getId() + ",i:" + temp);
        }
    });
}
```
<span style="color: red">总结：因为线程池大小为3，每个任务输出index后sleep 2秒，所以每两秒打印3个数字。定长线程池的大小最好根据系统资源进行设置。如Runtime.getRuntime().availableProcessors()。</span>


**newScheduledThreadPool**——创建一个定长线程池，支持定时及周期性任务执行。


```
// 延时3秒执行
ScheduledExecutorService newScheduledThreadPool = Executors.newScheduledThreadPool(5);
		for (int i = 0; i < 10; i++) {
			final int temp = i;
			newScheduledThreadPool.schedule(new Runnable() {
				public void run() {
					System.out.println("i:" + temp);
				}
			}, 3, TimeUnit.SECONDS);
}
```


**newSingleThreadExecutor**——创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。
```
ExecutorService newSingleThreadExecutor = Executors.newSingleThreadExecutor();
for (int i = 0; i < 10; i++) {
    final int index = i;
    newSingleThreadExecutor.execute(new Runnable() {
        @Override
        public void run() {
            System.out.println("index:" + index);
            try {
                Thread.sleep(200);
            } catch (Exception e) {
                // TODO: handle exception
            }
        }
    });
}
```
<span style="color: red">注意: 结果依次输出，相当于顺序执行各个任务。</span>


#### 线程池的工作原理？
![线程池](/images/Thread/ThreadPool.jpg)


判断线程池里的核心线程是否都在执行任务。如果不是(核心线程空闲或者还有核心线程没有被创建)则创建一个新的工作线程来执行任务。如果核心线程都在执行任务，则进入下个流程。


线程池判断工作队列是否已满。如果工作队列没有满，则将新提交的任务存储在这个工作队列里。如果工作队列满了，则进入下个流程。


判断线程池里的线程是否都处于工作状态。如果没有，则创建一个新的工作线程来执行任务。如果已经满了，则交给饱和策略来处理这个任务。


#### 如何合理的配置线程池？
<table>
    <tr>
        <th>角度</th>
        <th>类型</th>
        <th>解释</th>
    </tr>
    <tr>
        <td rowspan="3">任务的性质</td>
        <td style="width: 150px">CPU密集型任务</td>
        <td>配置尽可能少的线程数量，如配置Ncpu+1个线程的线程池</td>
    </tr>
    <tr>
        <td style="width: 150px">IO密集型任务</td>
        <td>由于需要等待IO操作，线程并不是一直在执行任务，则配置尽可能多的线程，如2*Ncpu</td>
    </tr>
    <tr>
        <td style="width: 150px">混合型任务</td>
        <td>如果可以拆分，则将其拆分成一个CPU密集型任务和一个IO密集型任务，只要这两个任务执行的时间相差不是太大，那么分解后执行的吞吐率要高于串行执行的吞吐率，如果这两个任务执行时间相差太大，则没必要进行分解。我们可以通过Runtime.getRuntime().availableProcessors()方法获得当前设备的CPU个数</td>
    </tr>
    <tr>
      <td>任务的优先级</td>
      <td style="width: 150px">高，中和低</td>
      <td>使用优先级队列PriorityBlockingQueue来处理。它可以让优先级高的任务先得到执行，需要注意的是如果一直有优先级高的任务提交到队列里，那么优先级低的任务可能永远不能执行</td>
    </tr>
    <tr>
        <td>任务的执行时间</td>
        <td style="width: 150px">长，中和短</td>
        <td>交给不同规模的线程池来处理，或者也可以使用优先级队列，让执行时间短的任务先执行</td>
    </tr>
    <tr>
        <td>任务的依赖性</td>
        <td style="width: 150px">是否依赖其他系统资源，如数据库连接</td>
        <td>依赖数据库连接池的任务，因为线程提交SQL后需要等待数据库返回结果，如果等待的时间越长CPU空闲时间就越长，那么线程数应该设置越大，这样才能更好的利用CPU</td>
    </tr>
</table>