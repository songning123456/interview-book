#### Mysql数据库优化？
添加适当的索引，索引对查询速度影响很大，必须添加索引。主键索引、唯一索引、普通索引、全文索引。


添加适当存储过程、触发器、事务等。


读写分离(主从数据库)。


对sql语句的一些优化(查询执行速度比较慢的sql语句)。


分表分区(分表把一张大表分成多张表；分区把一张表里面的分配到不同的区域存储)。


对mysql服务器硬件的升级操作。


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


不可重复读的重点是修改，幻读的重点在于新增或者删除。
