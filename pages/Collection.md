#### Java集合类框架的基本接口有哪些？
| 接口 | 解释 | 
| :----- | :----- | 
| Collection | 代表一组对象，每一个对象都是它的子元素  | 
| Set | 不包含重复元素的Collection | 
| List | 有顺序的collection，并且可以包含重复元素 | 
| Map | 可以把键(key)映射到值(value)的对象，键不能重复 | 


#### 为什么集合类没有实现Cloneable和Serializable接口？
集合类接口指定了一组叫做元素的对象。集合类接口的每一种具体的实现类都可以选择以它自己的方式对元素进行保存和排序。有的集合类允许重复的键，有些不允许。


#### 什么是迭代器(Iterator)？Iterator和ListIterator的区别是什么？
Iterator接口提供了很多对集合元素进行迭代的方法。每一个集合类都包含了可以返回迭代器实例的迭代方法。迭代器可以在迭代的过程中删除底层集合的元素。克隆(cloning)或者是序列化(serialization)的语义和含义是跟具体的实现相关的。因此，应该由集合类的具体实现来决定如何被克隆或者是序列化。


| Iterator | ListIterator |
| :----- | :----- |
| 可用来遍历Set和List集合 | <div style="width: 300px">只能用来遍历List</div> |
| 对集合只能是前向遍历 | <div style="width: 300px">既可以前向也可以后向</div> |
| 实现了Iterator接口，并包含其他的功能。比如增加元素，替换元素，获取前一个和后一个元素的索引等等 | <div style="width: 300px">————</div> |


#### 快速失败(fail-fast)和安全失败(fail-safe)的区别是什么？
Iterator的安全失败是基于对底层集合做拷贝，因此，它不受源集合上修改的影响。java.util包下面的所有的集合类都是快速失败的，而java.util.concurrent包下面的所有的类都是安全失败的。快速失败的迭代器会抛出ConcurrentModificationException异常，而安全失败的迭代器永远不会抛出这样的异常。


#### HashMap和Hashtable有什么区别？
| HashMap | Hashtable | 
| :----- | :----- | 
| 允许键和值是null | 不允许键或者值是null | 
| 非同步，适合于单线程环境 | 同步，适合于多线程环境 |
| 可供应用迭代的键的集合，因此，HashMap是快速失败的 | 对键的列举(Enumeration) |


#### <a href="https://www.jianshu.com/p/ee0de4c99f87">你知道HashMap的基本结构吗？</a>
![HashMap](/images/Collection/HashMap.jpg)


**数组+链表+红黑树**


```
// 红黑树的5个特性
每个节点或者是黑色，或者是红色；
根节点是黑色；
每个叶子节点是黑色；
如果一个节点是红色的，则它的子节点必须是黑色；
从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点。(这里指到叶子节点的路径)
```


数组中每个元素都是一个链表,HashMap是基于hash原理：我们对map进行put的时候，会对键调用hashcode方法，通过hashcode获得bucket，从而存储entry。 


#### ConcurrentHashMap实现线程安全的底层原理是什么？
![ConcurrentHashMap](/images/Collection/ConcurrentHashMap.jpeg)


**锁分段技术**——首先将数据分成一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问。有些方法需要跨段，比如size()和containsValue()，它们可能需要锁定整个表而而不仅仅是某个段，这需要按顺序锁定所有段，操作完毕后，又按顺序释放所有段的锁。这里“按顺序”是很重要的，否则极有可能出现死锁，在ConcurrentHashMap内部，段数组是final的，并且其成员变量实际上也是final的。但是，仅仅是将数组声明为final的并不保证数组成员也是final的，这需要实现上的保证。这可以确保不会出现死锁，因为获得锁的顺序是固定的。


ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。Segment是一种可重入锁ReentrantLock，在ConcurrentHashMap里扮演锁的角色，HashEntry则用于存储键值对数据。一个ConcurrentHashMap里包含一个Segment数组，Segment的结构和HashMap类似，是一种数组和链表结构，一个Segment里包含一个HashEntry数组，每个HashEntry是一个链表结构的元素， 每个Segment守护者一个HashEntry数组里的元素,当对HashEntry数组的数据进行修改时，必须首先获得它对应的Segment锁。


#### TreeMap结构，如何实现有序的？
TreeMap实现SortMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器。当用Iterator遍历TreeMap时，得到的记录是排过序的。TreeMap取出来的是排序后的键值对。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。TreeMap基于红黑树实现。TreeMap没有调优选项，因为该树总处于平衡状态。


```
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
Java提供了只包含一个compareTo()方法的Comparable接口。这个方法可以个给两个对象排序。具体来说，它返回负数，0，正数来表明输入对象小于，等于，大于已经存在的对象。


Java提供了包含compare()和equals()两个方法的Comparator接口。compare()方法用来给两个输入参数排序，返回负数，0，正数表明第一个参数是小于，等于，大于第二个参数。equals()方法需要一个对象作为参数，它用来决定输入参数是否和comparator相等。只有当输入参数也是一个comparator并且输入参数和当前comparator的排序结果是相同的时候，这个方法才返回true。
