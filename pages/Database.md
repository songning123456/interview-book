#### 数据库性能优化？
* 添加适当的索引，索引对查询速度影响很大，必须添加索引。主键索引、唯一索引、普通索引、全文索引。


* 对sql语句的一些优化(查询执行速度比较慢的sql语句)。


* 读写分离(主从数据库)。


* 分表分区(分表把一张大表分成多张表；分区把一张表里面的分配到不同的区域存储)。


* 对mysql服务器硬件的升级操作。


#### 数据库事务的特性？
| 特性 | 解释 | 
| :----- | :----- | 
| 原子性(Atomicity) | 事务的原子性指的是，事务中包含的程序作为数据库的逻辑工作单位，它所做的对数据修改操作要么全部执行，要么完全不执行，这种特性称为原子性 | 
| 一致性(Consistency) | 事务一致性值得是在一个事务执行之前和执行之后数据库都必须处于一致性状态(中途是否一致不用管)，这种特性称为一致性 | 
| 隔离性(Isolation) | 隔离性指的是并发的事务是相互隔离的。多个事务并发执行时，一个事务的执行不应影响其他事务的执行 | 
| 持久性(Durability) | 持久性指当系统或介质发生故障时，确保已提交的更新不能丢失 | 


#### 数据库的隔离级别？
| 隔离级别 | 脏读 | 不可重复读 | 幻读 | 
| :----- | :----- | :----- | :----- | 
| READ UNCOMMITED(读未提交) | √ | √ | √ | 
| READ COMMITED(读提交) | × | √ | √ | 
| REPEATABLE READ(可重复读) | × | × | √ | 
| SERIALIZABLE(串行化) | × | × | × | 


#### 什么是脏读、不可重复读、幻读？
| 名称 | 解释 | 
| :----- | :----- | 
| <div style="width: 100px">脏读</div> | 一个事务读取到了另一个事务未提交的数据操作结果，可能造成所有数据一起回滚 | 
| <div style="width: 100px">不可重复读</div> | 事务T1读取某一数据后，事务T2对其做了修改，当事务T1再次读该数据时得到与前一次不同的值。这样就发生了在一个事务内两次读到的数据是不一样的，因此称为是不可重复读 | 
| <div style="width: 100px">幻读</div> | 事务在操作过程中进行两次查询，第二次查询的结果包含了第一次查询中未出现的数据或者缺少了第一次查询中出现的数据(并不要求两次查询的SQL语句相同)。这是因为在两次查询过程中有另外一个事务插入数据造成的 | 


不可重复读的重点是**修改**，幻读的重点在于**新增**或者**删除**。


#### Mysql常有的索引有哪些？它们的区别是什么？
**Hash索引 && B+树索引**


* 如果是等值查询，那么哈希索引明显有绝对优势，因为只需要经过一次算法即可找到相应的键值；当然了，这个前提是，键值都是唯一的。如果键值不是唯一的，就需要先找到该键所在位置，然后再根据链表往后扫描，直到找到相应的数据。


* 如果是范围查询检索，这时候哈希索引就毫无用武之地了，因为原先是有序的键值，经过哈希算法后，有可能变成不连续的了，就没办法再利用索引完成范围查询检索。


* 同理，哈希索引也没办法利用索引完成排序，以及like 'xxx%'这样的部分模糊查询(这种部分模糊查询，其实本质上也是范围查询)。


* 哈希索引也不支持多列联合索引的最左匹配规则。


* B+树索引的关键字检索效率比较平均，不像B树那样波动幅度大，在有大量重复键值情况下，哈希索引的效率也是极低的，因为存在所谓的哈希碰撞问题。


#### <a href="https://www.cnblogs.com/wdss/p/11186411.html">Mysql索引查询失效的情况？</a>
* like以%开头，索引无效；当like前缀没有%，后缀有%时，索引有效；


* or语句前后没有同时使用索引。当or左右查询字段只有一个是索引，该索引失效，只有当or左右查询字段均为索引时，才会生效；


* 组合索引，不是使用第一列索引，索引失效；


* 数据类型出现隐式转化。如varchar不加单引号的话可能会自动转换为int型，使索引无效，产生全表扫描；


* 在索引字段上使用not，<>，!=。不等于操作符是永远不会用到索引的，因此对它的处理只会产生全表扫描。优化方法：key<>0 改为 key>0 or key<0；


* 对索引字段进行计算操作、字段上使用函数；


* 当全表扫描速度比索引速度快时，mysql会使用全表扫描，此时索引失效；


#### 常用SQL语句的优化？
* SELECT子句中避免使用 *，尽量应该根据业务需求按字段进行查询。


* 尽量多使用COMMIT，如对大数据量的分段批量提交释放了资源，减轻了服务器压力。


* 用UNION-ALL替换UNION，因为UNION-ALL不会过滤重复数据，所执行效率要快于UNION，并且UNION可以自动排序，而UNION-ALL不会。


* 避免在索引列上使用计算和函数，这样索引就不能使用。


* 用 >= 替换 >。


* 用 NOT EXISTS 或 (外连接+判断为空) 方案替换 NOT IN 操作符。


* LIKE操作符。


#### left join、inner join和right join的区别？
![join-method](/images/Database/join-method.jpg)


| 名称 | 解释 | 
| :----- | :----- | 
| left join | 返回包括左表中的所有记录和右表中联结字段相等的记录 | 
| inner join | 返回包括右表中的所有记录和左表中联结字段相等的记录 | 
| right join | 只返回两个表中联结字段相等的行 | 


![用户-课程表](/images/Database/test-join.PNG)


* **left join**


```sql
select user.id as user_id, user.user_name, class.id as class_id, class.class_name from user left join class on user.id = class.id
```
![left join](/images/Database/left-join.PNG)


* **right join**


```sql
select user.id as user_id, user.user_name, class.id as class_id, class.class_name from user right join class on user.id = class.id
```
![right join](/images/Database/right-join.PNG)


* **inner join**


```sql
select user.id as user_id, user.user_name, class.id as class_id, class.class_name from user inner join class on user.id = class.id
```
![inner join](/images/Database/inner-join.PNG)


#### 你知道Mysql主从复制的原理吗？
![主从复制](/images/Database/master-slave.png)


* master在每个事务更新数据完成之前，将该操作记录串行地写入到binlog文件中。


* slave开启一个I/O Thread，该线程在master打开一个普通连接，主要工作是binlog dump process。如果读取的进度已经跟上了master，就进入睡眠状态并等待master产生新的事件。I/O线程最终的目的是将这些事件写入到中继日志(relay log)中。


* SQL Thread会读取中继日志，并顺序执行该日志中的SQL事件，从而与主数据库中的数据保持一致。
