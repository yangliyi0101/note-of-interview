## sql语句 outer join中的on和where的区别
tips：数据库在通过连接两张或多张表来返回记录时，都会生成一张中间的临时表，然后再将这张临时表返回给用户。

on：是用来生成临时表时过滤的条件。由于left join表示把左表的数据全部显示，所以 on 只是用来限制右表，无论on后面跟多少个and、or、not，都无法对左表起作用。

where：是在生成完了临时表后，再对临时表进行过滤，没有左表右表的区别了。

如果是inner join，放 on 和放 where 产生的效果一样，没有说那种效率更高，推荐使用on，因为where使用的连接语句在数据库中称为隐形连接，慢慢地被淘汰了，而inner join...on子句产生的连接称为显性连接。 如果是 outer join就有区别了，on生效在前，where生效在后。

## 数据库优化
1） 选取最适用的字段： MySQL可以很好的支持大数据量的存取，但是一般说来，数据库中的表越小，在它上面执行的查询也就会越快。因此，在创建表的时候，为了获得更好的性能，我们可以将表中字段的宽度设得尽可能小。另外一个提高效率的方法是在可能的情况下，应该尽量把字段设置为NOT NULL，这样在将来执行查询的时候，数据库不用去比较NULL值。  

2）使用连接（JOIN）来代替子查询(Sub-Queries)。连接（JOIN）..之所以更有效率一些，是因为MySQL不需要在内存中创建临时表来完成这个逻辑上的需要两个步骤的查询工作。

3）使用联合(UNION)来代替手动创建的临时表。

4）事务。它的作用是：要么语句块中每条语句都操作成功，要么都失败。换句话说，就是可以保持数据库中数据的一致性和完整性。

5）锁定表。由于事务的独占性，有时候会影响数据库的性能。这时可以用锁定表来获取更好的性能。

	LOCK TABLE inventory WRITE SELECT quantity  FROM   inventory   WHERE Item='book';
	...
	UPDATE   inventory   SET   Quantity=11   WHERE  Item='book';UNLOCKTABLES
这里，我们用一个select语句取出初始数据，通过一些计算，用update语句将新值更新到表中。包含有WRITE关键字的LOCKTABLE语句可以保证在UNLOCKTABLES命令被执行之前，不会有其它的访问来对inventory进行插入、更新或者删除的操作。

6）使用外键。

7）使用索引。索引应建立在那些将用于JOIN,WHERE判断和ORDERBY排序的字段上。尽量不要对数据库中某个含有大量重复的值的字段建立索引。

## 参考 SQL执行顺序
(5)SELECT DISTINCT TOP(<top_specification>) <select_list>
(1)FROM <left_table> <join_type> JOIN <right_table> ON <on_predicate>
(2)WHERE <where_predicate>
(3)GROUP BY <group_by_specification>
(4)HAVING <having_predicate>
(6)ORDER BY <order_by_list>

T-SQL在查询各个阶级分别干了什么： （1）FROM 阶段 FROM阶段标识出查询的来源表，并处理表运算符。在涉及到联接运算的查询中（各种join），主要有以下几个步骤：

	a.求笛卡尔积。不论是什么类型的联接运算，首先都是执行交叉连接（cross join），求笛卡儿积，生成虚拟表VT1-J1。 

	b.ON筛选器。这个阶段对上个步骤生成的VT1-J1进行筛选，根据ON子句中出现的谓词进行筛选，让谓词取值为true的行通过了考验，插入到VT1-J2。 

	c.添加外部行。如果指定了outer join，还需要将VT1-J2中没有找到匹配的行，作为外部行添加到VT1-J2中，生成VT1-J3

经过以上步骤，FROM阶段就完成了。概括地讲，FROM阶段就是进行预处理的，根据提供的运算符对语句中提到的各个表进行处理（除了join，还有apply，pivot，unpivot）

（2）WHERE阶段 WHERE阶段是根据<where_predicate>中条件对VT1中的行进行筛选，让条件成立的行才会插入到VT2中。

（3）GROUP BY阶段 GROUP阶段按照指定的列名列表，将VT2中的行进行分组，生成VT3。最后每个分组只有一行。

（4）HAVING阶段 该阶段根据HAVING子句中出现的谓词对VT3的分组进行筛选，并将符合条件的组插入到VT4中。

（5）SELECT阶段 　　这个阶段是投影的过程，处理SELECT子句提到的元素，产生VT5。这个步骤一般按下列顺序进行 a.计算SELECT列表中的表达式，生成VT5-1。 b.若有DISTINCT，则删除VT5-1中的重复行，生成VT5-2 c.若有TOP，则根据ORDER BY子句定义的逻辑顺序，从VT5-2中选择签名指定数量或者百分比的行，生成VT5-3

（6）ORDER BY阶段 根据ORDER BY子句中指定的列明列表，对VT5-3中的行，进行排序，生成游标VC6.