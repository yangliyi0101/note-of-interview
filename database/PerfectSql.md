## sql语句 outter join中的on和where的区别
tips：数据库在通过连接两张或多张表来返回记录时，都会生成一张中间的临时表，然后再将这张临时表返回给用户。

on：是用来生成临时表时过滤的条件。由于left join表示把左表的数据全部显示，所以 on 只是用来限制右表，无论on后面跟多少个and、or、not，都无法对左表起作用。

where：是在生成完了临时表后，再对临时表进行过滤，没有左表右表的区别了。

如果是inner join，放 on 和放 where 产生的效果一样，没有说那种效率更高，推荐使用on，因为where使用的连接语句在数据库中称为隐形连接，慢慢地被淘汰了，而inner join...on子句产生的连接称为显性连接。 如果是 outer join就有区别了，on生效在前，where生效在后。

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