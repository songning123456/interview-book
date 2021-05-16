#### Java集合类框架的基本接口有哪些？
| 接口 | 解释 | 
| :----- | :----- | 
| Collection | 代表一组对象，每一个对象都是它的子元素。  | 
| Set | 不包含重复元素的Collection。 | 
| List | 有顺序的collection，并且可以包含重复元素。 | 
| Map | 可以把键(key)映射到值(value)的对象，键不能重复。 | 


#### 为什么集合类没有实现Cloneable和Serializable接口？
集合类接口指定了一组叫做元素的对象。集合类接口的每一种具体的实现类都可以选择以它自己的方式对元素进行保存和排序。有的集合类允许重复的键，有些不允许。


#### 什么是迭代器(Iterator)？Iterator和ListIterator的区别是什么？
Iterator接口提供了很多对集合元素进行迭代的方法。每一个集合类都包含了可以返回迭代器实例的迭代方法。迭代器可以在迭代的过程中删除底层集合的元素。克隆(cloning)或者是序列化(serialization)的语义和含义是跟具体的实现相关的。因此，应该由集合类的具体实现来决定如何被克隆或者是序列化。


| Iterator | ListIterator |
| :----- | :----- |
| 可用来遍历Set和List集合。 | <div style="width: 300px">只能用来遍历List。</div> |
| 对集合只能是前向遍历。 | <div style="width: 300px">既可以前向也可以后向。</div> |
| 实现了Iterator接口，并包含其他的功能。比如增加元素，替换元素，获取前一个和后一个元素的索引等等。 | <div style="width: 300px">————</div> |


#### 快速失败(fail-fast)和安全失败(fail-safe)的区别是什么？
Iterator的安全失败是基于对底层集合做拷贝，因此它不受源集合上修改的影响。java.util包下面的所有的集合类都是快速失败的，而java.util.concurrent包下面的所有的类都是安全失败的。快速失败的迭代器会抛出ConcurrentModificationException异常，而安全失败的迭代器永远不会抛出这样的异常。


#### HashMap和Hashtable有什么区别？
| HashMap | Hashtable | 
| :----- | :----- | 
| 允许键和值是null。 | 不允许键或者值是null。 | 
| 非同步，适合于单线程环境。 | 同步，适合于多线程环境。 |
| 可供应用迭代的键的集合，因此，HashMap是快速失败的。 | 对键的列举(Enumeration)。 |


#### <a href="https://www.jianshu.com/p/ee0de4c99f87">你知道HashMap的基本结构吗？讲一下HashMap的put的过程？</a>
![HashMap](/images/Collection/HashMap.jpg)


**数组+链表+红黑树**


* 对key求Hash值，然后再计算下标；
* 如果没有碰撞，直接放入桶中；
* 如果碰撞了，以链表的方式链接到后面；
* 如果链表长度超过阀值(TREEIFY_THRESHOLD == 8)，就把链表转成红黑树；
* 如果节点已经存在就替换旧值；
* 如果桶满了(容量 * 加载因子)，就需要resize。



**红黑树的5个特性**


* 每个节点或者是黑色，或者是红色；
* 根节点是黑色；
* 每个叶子节点是黑色；
* 如果一个节点是红色的，则它的子节点必须是黑色；
* 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点(这里指到叶子节点的路径)。


**扩容后，原始值所在的位置**


元素的下标要么**不变**，要么变为**原下标+原容量**。


#### ConcurrentHashMap实现线程安全的底层原理是什么？
![ConcurrentHashMap](/images/Collection/ConcurrentHashMap.jpeg)


**锁分段技术**


首先将数据分成一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问。有些方法需要跨段，比如size()和containsValue()，它们可能需要锁定整个表而而不仅仅是某个段，这需要按顺序锁定所有段，操作完毕后，又按顺序释放所有段的锁。这里“按顺序”是很重要的，否则极有可能出现死锁，在ConcurrentHashMap内部，段数组是final的，并且其成员变量实际上也是final的。但是，仅仅是将数组声明为final的并不保证数组成员也是final的，这需要实现上的保证。这可以确保不会出现死锁，因为获得锁的顺序是固定的。


ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。Segment是一种可重入锁ReentrantLock，在ConcurrentHashMap里扮演锁的角色，HashEntry则用于存储键值对数据。一个ConcurrentHashMap里包含一个Segment数组，Segment的结构和HashMap类似，是一种数组和链表结构，一个Segment里包含一个HashEntry数组，每个HashEntry是一个链表结构的元素， 每个Segment守护者一个HashEntry数组里的元素,当对HashEntry数组的数据进行修改时，必须首先获得它对应的Segment锁。


#### TreeMap结构，如何实现有序的？
TreeMap实现SortMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器。当用Iterator遍历TreeMap时，得到的记录是排过序的。TreeMap取出来的是排序后的键值对。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。TreeMap基于红黑树实现。TreeMap没有调优选项，因为该树总处于平衡状态。


```java
// 测试用例
Map<String, Integer> treeMap = new TreeMap<>();
treeMap.put("3", 3);
treeMap.put("2", 2);
treeMap.put("1", 1);
treeMap.put("4", 4);
for (Map.Entry<String, Integer> item : treeMap.entrySet()) {
    String key = item.getKey();
    Integer value = item.getValue();
    System.out.println("key -> " + key + "; value -> " + value);
}
```


```
// 运行结果
key -> 1; value -> 1
key -> 2; value -> 2
key -> 3; value -> 3
key -> 4; value -> 4
```


#### Comparable和Comparator接口是干什么的？列出它们的区别？
* **Comparable实现**


Comparable是排序接口。若一个类实现了Comparable接口，就意味着`该类支持排序`。此外，实现Comparable接口的类的对象可以用作有序映射(如TreeMap)中的键或有序集合(TreeSet)中的元素，而不需要指定比较器。接口中通过x.compareTo(y)来比较x和y的大小。若返回负数，意味着x < y；返回零，意味着x = y；返回正数，意味着x > y。      


```java
// Person对象
public class Person implements Comparable<Person> {
    private String name;
    private int age;
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    @Override
    public int compareTo(Person person) {
        // this.age - person.age升序，person.age - this.age降序
        return this.age - person.age;
    }
    @Override
    public String toString() {
        return this.name + "-" + this.age;
    }
}

// Comparable实现
public class ComparableMain {
    public static void main(String[] args) {
        ArrayList<Person> list = new ArrayList<>();
        list.add(new Person("ccc", 20));
        list.add(new Person("AAA", 30));
        list.add(new Person("bbb", 10));
        list.add(new Person("ddd", 40));
        System.out.println("Comparable-before: " + list);
        Collections.sort(list);
        System.out.println("Comparable-after: " + list);
    }
}

// 结果
Comparable-before: [ccc-20, AAA-30, bbb-10, ddd-40]
Comparable-after: [bbb-10, ccc-20, AAA-30, ddd-40]
```


* **Comparator实现**


Comparator是比较器接口。我们若需要控制某个类的次序，而该类本身不支持排序(即没有实现Comparable接口)；那么，我们可以建立一个`该类的比较器`来进行排序。这个比较器只需要实现Comparator接口即可。也就是说，我们可以通过`实现Comparator类来新建一个比较器`，然后通过该比较器对类进行排序。int compare(T o1, T o2)和上面的x.compareTo(y)类似，定义排序规则后返回。正数，零和负数分别代表大于、等于和小于。


```java
// Person略，去掉implements Comparable<Person>
...

// 升序
public class AscComparator implements Comparator<Person> {
    @Override
    public int compare(Person person1, Person person2) {
        return person1.getAge() - person2.getAge();
    }
}

// 降序
public class DescComparator implements Comparator<Person> {
    @Override
    public int compare(Person person1, Person person2) {
        return person2.getAge() - person1.getAge();
    }
}

// Comparator实现
public class ComparatorMain {
    public static void main(String[] args) {
        ArrayList<Person> list = new ArrayList<>();
        list.add(new Person("ccc", 20));
        list.add(new Person("AAA", 30));
        list.add(new Person("bbb", 10));
        list.add(new Person("ddd", 40));
        System.out.println("Comparator-before: " + list);
        AscComparator ascComparator = new AscComparator();
        list.sort(ascComparator);
        System.out.println("Comparator-asc: " + list);
        DescComparator descComparator = new DescComparator();
        list.sort(descComparator);
        System.out.println("Comparator-desc: " + list);
    }
}

// 结果
Comparator-before: [ccc-20, AAA-30, bbb-10, ddd-40]
Comparator-asc: [bbb-10, ccc-20, AAA-30, ddd-40]
Comparator-desc: [ddd-40, AAA-30, ccc-20, bbb-10]
```