#### mysql有哪些索引?
* **普通索引**
```
CREATE INDEX indexName ON mytable(username(length));
ALTER mytable ADD INDEX [indexName] ON (username(length));
DROP INDEX [indexName] ON mytable; 
```


* **唯一索引**
```
CREATE UNIQUE INDEX indexName ON mytable(username(length));
ALTER mytable ADD UNIQUE [indexName] ON (username(length));
```
索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。


* **主键索引**
```
CREATE TABLE mytable(  
ID INT NOT NULL,   
username VARCHAR(16) NOT NULL,  
PRIMARY KEY(ID)
);
```
它是一种特殊的唯一索引，不允许有空值。(一个表只能有一个主键)


* **组合索引**
```
ALTER TABLE mytable ADD INDEX name_city_age (name(10),city,age); 
```


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
  
