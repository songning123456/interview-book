#### 简单讲一下java的跨平台原理？
![Java](/images/JVM/PlateForm.jpg)


由于各操作系统(windows、linux等)支持的指令集，不是完全一致的。就会让我们的程序在不同的操作系统上要执行不同程序代码。Java开发了适用于不同操作系统及位数的Java虚拟机来屏蔽个系统之间的差异，提供统一的接口。对于我们java开发者而言，你只需要在不同的系统上安装对应的不同java虚拟机，这时你的java程序只要遵循java规范，就可以在所有的操作系统上面运行java程序了。Java通过不同的系统、不同版本、不同位数的Java虚拟机(jvm)，来屏蔽不同的系统指令集差异而对外体统统一的接口(java API)，对于我们普通的java开发者而言，只需要按照接口开发即可。如果我系统需要部署到不同的环境时，只需在系统上面按照对应版本的虚拟机即可。


#### 你知道JDK、JRE和JVM三者之间的关系吗？
![Relation](/images/JVM/Relation.jpg)


| 名称 | 解释 | 
| :----- | :----- | 
| <div style="width: 300px">JDK(Java Development Kit)</div> | 整个JAVA的核心，包括了Java运行环境JRE(Java Runtime Environment)、一堆Java工具(javac/java/jdb等)和Java基础的类库(即Java API 包括rt.jar)。 | 
| <div style="width: 300px">JRE(Java Runtime Environment)</div> | 运行基于Java语言编写的程序所不可缺少的运行环境。 | 
| <div style="width: 300px">JVM(Java Virtual Machine)</div> | Java虚拟机，它是整个Java实现跨平台的最核心的部分。 | 


#### 在Java中，有哪些对象可以被垃圾回收？
1. 对象没有引用(obj=null)。
```
思考：如果对象的引用被置为null，垃圾收集器是否会立即释放对象占用的内存？
答：不会，在下一个垃圾回收周期中，这个对象才会被被回收。
```
2. 作用域发生未捕获异常。
3. 程序在作用域正常执行完毕。
4. 程序执行了System.exit()。
5. 程序发生意外终止(被杀进程等)。


JVM的内存其实是有限制的，不可能是无限的，昂贵的资源，2核4G的机器，堆内存也就2GB左右，4核8G的机器，堆内存可能也就4G左右，栈内存也需要空间，metaspace区域放类信息也需要空间。在jvm里必然是有一个内存分代模型，年轻代和老年代。给年轻代一共是2GB内存，给老年代是2GB内存，默认情况下eden和2个s的比例：8:1:1，eden是1.6GB，S是0.2GB。如果说eden区域满了，此时必然触发垃圾回收，young gc(ygc)，没有人引用的对象就是垃圾对象。


#### 方法区中的数据什么时候被回收？
同时满足以下3个条件: 
1. 类的所有实例都已经被回收。
2. 加载类的ClassLoader已经被回收。
3. 类对象的Class对象没有被引用。(即没有通过反射引用该类的地方。)


#### 你知道内存泄露和内存溢出的区别吗？
| 定义 | 解释 | 
| :----- | :----- | 
| <div style="width: 100px">内存泄露</div> | 应用程序在申请内存后，无法释放已经申请的内存空间。一次内存泄露危害可以忽略，但如果任其发展最终会导致内存溢出(out of memory)。如读取文件后流要进行及时的关闭以及对数据库连接的释放。 | 
| <div style="width: 100px">内存溢出</div> | 指应用程序在申请内存时，没有足够的内存空间供其使用。如我们在项目中对于大批量数据的导入，采用分段批量提交的方式。 | 


#### Java类加载机制？
👉 [Java类加载机制](https://blog.csdn.net/weixin_40236948/article/details/88072698)


类加载器是一个用来加载类文件的类，类加载的原理基于三个机制：委托、可见性和单一性。


| 机制 | 解释 | 
| :----- | :----- | 
| <div style="width: 150px">委托机制</div> | 将加载一个类的请求交给父类加载器，如果这个父类加载器不能够找到或者加载这个类，那么再加载它。 | 
| <div style="width: 150px">可见性</div> | 子类的加载器可以看见所有父类的加载器加载的类，而父类加载器看不到子类加载器加载的类。 | 
| <div style="width: 150px">单一性</div> | 加载一个类一次，这是有委托机制确保子类加载器不会再次加载父类加载器加载过的类。 | 


| 预定义类加载器 | 解释 | 
| :----- | :----- | 
| <div style="width: 200px">启动类加载器(BootStrap)</div> | 引导类装入器是用本地代码实现的类装入器，它负责将Java_Runtime_Home/lib下面的核心类库或-Xbootclasspath选项指定的jar包加载到内存中。 | 
| <div style="width: 200px">扩展类加载器(Extension)</div> | 扩展类加载器是用Sun的ExtClassLoader实现的，他负责将Java_Runtime_Home/lib/ext或者是由系统变量-Djava.ext.dir指定位置中的类库加载到内存中。 | 
| <div style="width: 200px">系统类加载器(System)</div> | 由Sun的AppClassLoader实现的，负责将系统类路径java-classpath或者-Djava.class.path变量所指定的目录下的类库加载到内存中。 | 


#### Java代码执行顺序？
父类静态代码块 -> 子类静态代码块 -> 父类构造代码块 -> 父类构造函数 -> 子类构造代码块 -> 子类构造函数。


简单来说就是先执行静态代码块，再执行父类的构造代码块和构造函数，最后执行子类的构造代码块和构造函数。


#### Java虚拟机的运行时数据区有几块？线程私有和线程共享区域有哪些？
![JVM](/images/JVM/JVM.jpeg)


| 区域 | 私有/公有 | 解释 | 
| :----- | :----- | :----- | 
| <div style="width: 100px">程序计数器</div> | <div style="width: 100px">私有</div> | 当前线程执行的字节码的行号指示器。 | 
| <div style="width: 100px">Java虚拟机栈</div> | <div style="width: 100px">私有</div> | 存放基本数据类型、对象引用和returnAddress类型。 | 
| <div style="width: 100px">本地方法栈</div> | <div style="width: 100px">私有</div> | 为虚拟机使用到的Native方法服务。 | 
| <div style="width: 100px">Java堆</div> | <div style="width: 100px">公有</div> | 存放对象的实例，也是GC回收器管理的主要区域。 | 
| <div style="width: 100px">方法区</div> | <div style="width: 100px">公有</div> | 存放已被虚拟机加载的类信息、常量、静态变量、即时编译后的代码等数据。 | 
| <div style="width: 100px">运行时常量池</div> | <div style="width: 100px">————</div> | 方法区的一部分，存放编译期生成的各种字面量和符号引用。 | 
| <div style="width: 100px">直接内存</div> | <div style="width: 100px">————</div> | 不是虚拟机运行时数据区的一部分，也不是Java虚拟机规范中定义的内存区域，容易引起OOM异常，NIO会调用，不受Java堆大小的限制。 | 


#### 你知道JVM是如何运行起来的吗？我们的对象是如何分配的？
![JVMRun](/images/JVM/JVMRun.png)


有一个类里面包含了一个main方法，你去执行这个main方法，此时会启动一个jvm进程，他会默认就会有一个main线程，这个main线程就负责执行这个main方法的代码，进而创建各种对象。一般tomcat，类都会加载到jvm里去，spring容器而言都会对我们的类进行实例化成bean，有工作线程会来执行我们的bean实例对象里的方法和代码，进而也会创建其他的各种对象，实现业务逻辑。


#### 说说JVM的年轻代和老年代？它们各自使用了什么垃圾收集算法？
![HeapSpace](/images/JVM/HeapSpace.jpg)


年轻代对象因为生命周期短，每次有约90%以上对象的占用空间被回收，采用`复制-清除`算法清理，具体过程：将新生代分为一个Eden空间和两个Survivor空间，默认Eden空间和Survivor空间的比例为8:1，对象分配到Eden和其中一个Survivor空间，回收时将存活的对象复制到另一个Survivor空间，然后将Eden空间和先前使用的Survivor空间清理。


老年代因对象生命周期较长，每次回收只有少部分对象没清理，如果使用“复制-清理”算法的话需要额外预留更多的空闲空间用于复制生存对象(例如，100M的老年代占用空间每次能回收50%，那么他需要预留50M的空间，内存使用上不经济)，所以回收时使用`标记-整理`算法。


#### Java对象如何从年轻代转移到老年代？
1. 通过年龄计数器判断一个对象是否需要转移，对象每经过一个GC仍然活着，年龄计数器加1，当年龄超过设定的值(-XX:MaxTenuringThreshold=15)，则将其通过担保机制转移到老年代。
2. 也可以动态判定，当Survivor中年龄相同的对象超半数，则年龄大于该年龄的对象转移到老年代，无需等待到达设置的最大年龄值。
3. 大对象直接进入老年代。


#### Java的双亲委派机制？
👉 [通俗易懂的双亲委派机制](https://blog.csdn.net/codeyanbao/article/details/82875064)


#### JVM中哪些类可以作为GCRoot对象？
👉 [JVM中哪些类可以作为GCRoot对象](https://blog.csdn.net/qq_38545713/article/details/103492017)


1. java虚拟机栈中的引用的对象。
2. 方法区中的类静态属性引用的对象。(一般指被static修饰的对象，加载类的时候就加载到内存中。)
3. 方法区中的常量引用的对象。
4. 本地方法栈中的JNI(native方法)引用的对象。


#### G1回收器何时回收垃圾？
👉 [【原创】面试官问我G1回收器怎么知道你是什么时候的垃圾？](https://www.cnblogs.com/thisiswhy/p/12388638.html)