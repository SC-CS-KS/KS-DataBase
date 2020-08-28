# Hierarchy Modeling  

* 树形聚合Tree Aggregation

树形或是任意的图（需反规格化）可以被直接打成一条记录或文档存放，当树形结构被一次性取出时这会非常有效率。
如：我们需要展示一个blog的树形评论，搜索和任何存取这个实体都会存在问题。
对于大多数NoSQL的实现来说，更新数据都是很不经济的（相比起独立结点来说）

适用性
Key-Value 键值对数据库, Document Databases 文档数据库

* 邻接列表 Adjacency Lists
	
Adjacency Lists 邻接列表是一种图。  
每一个结点都是一个独立的记录，其包含了所有的父结点或子结点
这样，我们就可以通过给定的父或子结点来进行搜索。
当然，我们需要通过hop查询遍历图，这个技术在广度和深度查询，以及得到某个结点的子树上没有效率。

适用性
Key-Value 键值对数据库, Document Databases 文档数据库

* Materialized Paths

Materialized Paths 可以帮助避免递归遍历（如：树形结构），这个技术也可以被认为是反规格化的一种变种。
其想法是为每个结点加上父结点或子结点的标识属性，这样就可以不需要遍历就知道所有的后裔结点和祖先结点了。
这个技术对于全文搜索引擎来说非常有帮助，因为其可以允许把一个层级结构转成一个文档。

Materialized Paths 可以存储一个ID的集合，或是一堆ID拼出的字符串，后者允许你通过一个正则表达式来搜索一个特定的分支路径。

适用性
Key-Value 键值对数据库, Document Databases 文档数据, Search Engines 搜索引擎

* 嵌套集 Nested Sets  
	
Nested sets 嵌套集是树形结构的标准技术，它被广泛地用在了关系性数据库中。
它完全地适用于 Key-Value 键值对数据库 和 Document Databases 文档数据库，
这个技术的想法是把叶子结点存储成一个数组，并通过使用索引的开始和结束来映射每一个非叶子结点到一个叶子结点集

这样的数据结构对于immutable data不变的数据 有非常不错的效率
因为其点内存空间小，并且可以很快地找出所有的叶子结点而不需要树的遍历
尽管如此，在插入和更新上需要很高的性能成本，因为新的叶子结点需要大规模地更新索引。

适用性
Key-Value Stores 键值数据库, Document Databases 文档数据库

* 嵌套文档扁平化

有限的字段名 Nested Documents Flattening: Numbered Field Names		
搜索引擎基本上来说和扁平文档一同工作
如：每一个文档是一个扁平的字段和值的例表。
这种数据模型的用来把业务实体映射到一个文本文档上，如果你的业务实体有很复杂的内部结构，这可能会变得很有挑战。
一个典型的挑战是把一个有层级的文档映映射出来，例如，文档中嵌套另一个文档。
		
适用性
Search Engines 全文搜索

* 邻近查询 Nested Documents Flattening: Proximity Queries

这个技术用来解决扁平层次文档，它用邻近的查询来限制可被查询的单词的范围。
所有的技能和等级被放在一个字段中，叫 SkillAndLevel。
查询中出现的 “Excellent” 和 “Poetry” 必需一个紧跟另一个

适用性
Search Engines 全文搜索

* 图结构批处理 Batch Graph Processing
	
Graph databases 图数据库
如 neo4j 是一个出众的图数据库。
尤其是使用一个结点来探索邻居结点，或是探索两个或少量结点前的关系
但是处理大量的图数据是很没有效率的，因为图数据库的性能和扩展性并不是其目的

分布式的图数据处理可以被 MapReduce 和 Message Passing pattern 来处理
这个方法可以让 Key-Value stores, Document databases，和 BigTable-style databases 适合于处理大图。
	
Applicability
Key-Value Stores, Document Databases, BigTable-style Databases
