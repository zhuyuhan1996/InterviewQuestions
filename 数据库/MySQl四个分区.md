##### MySQl表的四种分区类型
1. 什么是表分区

就是将表根据条件分割成若干个小表。

2. 为什么要对表进行分区

为了改善大型表以及具有各种访问模式的表的可伸缩性，可管理型和提高数据库效率。

分区的一些优点：

- 与单个磁盘或文件系统分区相比，可以存储更多的数据。
- 对于那些已经失去保护意义的数据，通常可以通过删除与那些数据有关的分区，很容易的删除那些数据。相反地，在某些情况下，添加新数据的过程又可以通过为那些新数据专门增加一个新的分区，来方便的实现。
- 一些查询得到了极大的优化，这主要是借助于满足一个给定WHERE语句的数据可以只保存在一个或者多个分区内，这样在查找的时候就不用查找剩余的分区。
- count(),sum()这样的聚合函数，可以很同意的进行并行处理。
- 通过跨多个磁盘来分散数据查询，来获得更大的查询吞吐量。
3. 分区类型

**RANGE分区**：*基于属于一个给定连续区间的列值，把多行分配给分区。*

这些区间连续且不能重叠，使用values less than来进行定义。

```
PARTITION BY RANGE (store_id) (
    PARTITION p0 VALUES LESS THAN (6),
    PARTITION p1 VALUES LESS THAN (11),
    PARTITION p2 VALUES LESS THAN (16),
    PARTITION p3 VALUES LESS THAN MAXVALUE
);
```
RANGE分区在如下场合特别有用：
- 1）、当需要删除一个分区上的“旧的”数据时,只删除分区即可。如果你使用上面最近的那个例子给出的分区方案，你只需简单地使用”ALTER TABLE employees DROP PARTITION p0;”来删除所有在1991年前就已经停止工作的雇员相对应的所有行。对于有大量行的表，这比运行一个如”DELETE FROM employees WHERE YEAR (separated) <= 1990;”这样的一个DELETE查询要有效得多。
-  2）、想要使用一个包含有日期或时间值，或包含有从一些其他级数开始增长的值的列。
-  3）、经常运行直接依赖于用于分割表的列的查询。例如，当执行一个如”SELECT COUNT(*) FROM employees WHERE YEAR(separated) = 2000 GROUP BY store_id;”这样的查询时，MySQL可以很迅速地确定只有分区p2需要扫描，这是因为余下的分区不可能包含有符合该WHERE子句的任何记录。

**LIST分区**：*类似于按RANGE分区，区别在于LIST分区是基于列值匹配一个离散值集合中的某个值来进行选择。*

通过partition by list(expr)来实现，其中“expr”是某列值或一个基于某个列值、并返回一个整数值的表达式，然后通过“VALUES IN (value_list)”的方式来定义每个分区，其中“value_list”是一个通过逗号分隔的整数列表。

```
PARTITION BY LIST(store_id)
    PARTITION pNorth VALUES IN (3,5,6,9,17),
    PARTITION pEast VALUES IN (1,2,10,11,19,20),
    PARTITION pWest VALUES IN (4,12,13,14,18),
    PARTITION pCentral VALUES IN (7,8,15,16)
);
```
【要点】：如果试图插入列值（或分区表达式的返回值）不在分区值列表中的一行时，那么“INSERT”查询将失败并报错。

LIST分区除了能和RANGE分区结合起来生成一个复合的子分区，与HASH和KEY分区结合起来生成复合的子分区也是可能的。

**HASH分区**：*基于用户定义的表达式的返回值来进行选择的分区，该表达式使用将要插入到表中的这些行的列值进行计算。这个函数可以包含MySQL中有效的、产生非负整数值的任何表达式*

要使用HASH分区来分割一个表，要在CREATE TABLE 语句上添加一个“PARTITION BY HASH (expr)”子句，其中“expr”是一个返回一个整数的表达式。它可以仅仅是字段类型为MySQL整型的一列的名字。此外，你很可能需要在后面再添加一个“PARTITIONS num”子句，其中num是一个非负的整数，它表示表将要被分割成分区的数量

```
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY HASH(store_id)
PARTITIONS 4;
```
**LINE HASH**：线性哈希功能使用的一个线性的2的幂（powers-of-two）运算法则，而常规哈希使用的是求哈希函数值的模数。线性哈希分区和常规哈希分区在语法上的唯一区别在于，在“PARTITION BY”子句中添加“LINEAR”关键字。

**KSY分区**：*类似于按HASH分区，区别在于KEY分区只支持计算一列或多列，且MySQL服务器提供其自身的哈希函数。必须有一列或多列包含整数值*

在KEY分区中使用关键字LINEAR和在HASH分区中使用具有同样的作用，分区的编号是通过2的幂（powers-of-two）算法得到，而不是通过模数算法

```
CREATE TABLE tk (
    col1 INT NOT NULL,
    col2 CHAR(5),
    col3 DATE
)
PARTITION BY LINEAR KEY (col1)
PARTITIONS 3;
```