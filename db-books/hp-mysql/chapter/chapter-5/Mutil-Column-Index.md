# 多列索引

```md
一种常见的错误认识是“把WHERE条件上的列都建上索引”。
实际上，在多个列上创建独立的单列索引大部分情况下并不能提升MySQL的性能。
```
## “索引合并”（index merge）策略
```md
5.0版本引入，一定程度上可以使用表上的多个单列索引来定位指定的行。
更早版本的MySQL只能使用某一个单列索引，然而这种情况下没有哪一个单列索引是高效的。

表 sakila.film_actor 在字段film_id, actor_id 都有单列索引。
但对于以下查询两个索引都不是很好的选择：
SELECT film_id, actor_id from sakila.film_actor WHERE actor_id = 1 OR film_id = 1;
5.0版本之前 会使用全表扫描，除非改成UNION的方式。
5.0以后，能够同时使用这两个索引，并将结果进行合并。实际使用两个索引扫描的联合。
通过EXPLAIN 的Extra列可以看出来。（Using union(PRIMARY,idx_fk_film_id); Using where）
```

```md
索引合并策略有时候是一种优化的结果，但实际上更多时候说明了表上的索引建的很糟糕：
* 当服务器对第一个索引做相交操作时（通常有多个AND条件），意味着需要一个包含所有相关列的多列索引。
* 当服务器对第一个索引做联合操作时（通常有多个OR条件），通常需要耗费大量CPU和内存资源在算法的缓存、排序和合并操作上。
  特别是当索引的选择性不高时，需要合并扫描返回的大量数据。
* 重要的是，优化器不会把这些计算到“查询成本”中，优化器只关系随机页面的读取。
  这会使得查询的成本被“低估”，导致该执行计划还不如直接做全表扫描。
```

```md
如果在EXPLAIN中看到有索引合并，应该要检查下查询和表的结构，看是不是已经最优了。
也可以通过参数 optimizer_switch 来关闭索引合并功能。
也可以使用 IGNORE INDEX 提示让优化器忽略掉某些索引。
```

## 选择合适的索引列顺序
```md
在一个多列 B-Tree 索引中，所有列的顺序意味着索引首先按照最左列进行排序，其次是第二列，等等。
所以，索引可以按照升序或降序进行扫描，以满足精确符合列顺序的ORDER BY、GROUP BY 和 DISTINCT等子句的查询。

多列索引的列顺序至关重要，在“三星系统”中，列顺序也决定了一个索引能否能够成为一个正在的“三星索引”。
```
* 选择索引的列顺序的经验法则
> <font color=DarkSalmon size=3>将选择性最高的列放在索引的最前列</font>
```md
这个经验并不总是奏效。当不考虑排序和分组时，这个经验通常是好的。
这时索引的作用只是用于优化WHERE条件的查找。

然而查询效率不光依赖索引列的选择性，也和数据的分布有关。
所以可能需要根据那些运行频率最高的查询来调整索引列的顺序。
```
* 实例
```md
SELECT * FROM payment WHERE staff_id = 2 AND costumer_id = 584;

对于上述查询时创建一个（staff_id, costumer_id）索引，还是顺序颠倒下？
先预测下各个WHERE条件的分支对应的数据基数有多大：
(某些优化极客geek称之为IE"sarg"，是“可搜索参数（searchable argument）”的缩写)

SELECT SUM(staff_id = 2), SUM(costumer_id = 584) FROM payment\G
SUM(staff_id = 2): 7992
SUM(costumer_id = 584): 30
根据结果，可以将数量较小的字段放在前面，即customer_id。

再看看对于 costumer_id 的条件值，对于的 staff_id列的选择性。
SELECT SUM(staff_id = 2) FROM payment WHERE costumer_id = 584\G
SUM(staff_id = 2): 17

需要注意的是，查询的结果非常依赖于选定的具体指。
如果按照上述办法优化，可能对其他一些条件值的查询不公平，
服务器的整体性能可能更糟，或者某些查询运行变得不如预期。

如果从 pt-query-digest 工具的报告中提取“最差”查询，那么再按照上述办法选定索引顺序往往是高效的。
如果没有具体查询来运行，那么最好还是按经验法则来，因为经验法则考虑的是全局基数和选择性，而不是具体某个查询。
```
* 特殊情况
```md
当使用前缀索引，在某些条件值的基数比正常值高的时候，问题就来了。
如：
对应于没有登录的用户，用户名记录为“guest”，“guest”就称为了一个特殊的用户。
一旦涉及这个用户的查询，就和普通用户的查询大不一样，因为通常会有很多这样的数据存在。

系统账号也会存在类似问题，管理员账号问题。管理员账号的查询常常引发服务器性能问题。

解决办法：
修改应用程序代码，区分特殊用户和组，禁止针对这类用户和组执行某些查询。
```
* 小结
```md
尽管关于选择性和技术的经验法则值得去研究和分析，但是别忘了WHERE子句中排序、分组 和 范围条件等其它因素。
这些因素可能对查询性能造成非常大的影响。
```