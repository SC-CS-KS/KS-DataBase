# General Modeling
```md
原子聚合 Atomic Aggregates
	很多NoSQL的数据库（并不是所有）在事务处理上都是短板
		在某些情况下，他们可以通过分布式锁技术或是应用层管理的MVCC技术来实现其事务性
		但是，通常来说只能使用聚合Aggregates技术来保证一些ACID原则。
	这就是为什么我们的关系型数据库需要有强大的事务处理机制
		因为关系型数据库的数据是被规格化存放在了不同的地方。
	所以，Aggregates聚合允许我们把一个业务实体存成一个文档、存成一行，存成一个key-value，这样就可以原子式的更新了
	当然，原子聚合 Atomic Aggregates 这种数据模型并不能实现完全意义上的事务处理
		但是如果支持原子性，锁，或 test-and-set 指令，那么， Atomic Aggregates 是可以适用的
	适用性
		Key-Value Store 键值对数据库， Document Databases文档数据库， BigTable风格的数据库。
可枚举键 Enumerable Keys
	对于无顺序的Key-Value最大的好处是业务实体可以被容易地hash以分区在多个服务器上。
		而排序了的key会把事情搞复杂，但是有些时候，一个应用能从排序key中获得很多好处，就算是数据库本身不提供这个功能。
	来思考下email消息的数据模型
		一些NoSQL的数据库提供原子计数器以允许生一些连续的ID
			在这种情况下，我们可以使用 userID_messageID 来做为一个组合key
			如果我们知道最新的message ID，就可以知道前一个message，也可能知道再前面和后面的Message。
		Messages可以被打包
			比如，每天的邮件包。这样，我们就可以对邮件按指定的时间段来遍历。
	适用性
		Key-Value Store 键值对数据库。
降维 Dimensionality Reduction
	可以允许把一个多维的数据映射成一个Key-Value或是其它非多维的数据模型。
	传统的地理位置信息系统使用一些如“四分树QuadTree” 或 “R-Tree” 来做地理位置索引
		这些数据结构的内容需要被在适当的位置更新，并且，如果数据量很大的话，操作成本会很高。
		另一个方法是我们可以遍历一个二维的数据结构并把其扁平化成一个列表。
			一个众所周知的例子是Geohash（地理哈希）。
			一个Geohash使用“之字形”的路线扫描一个2维的空间，而且遍历中的移动可以被简单地用0和1来表示其方向，然后在移动的过程中产生0/1串。
			Geohash的最强大的功能是使用简单的位操作就可以知道两个区域间的距离
		Geohash把一个二维的坐标生生地变成了一个一维的数据模型，这就是降维技术。
	适用性
		Key-Value Store 键值对数据库， Document Databases文档数据库， BigTable风格的数据库。
索引表 Index Table
	可以在不支持索引的数据库中得到索引的好处。
		BigTable是这类最重要的数据库
	需要维护一个有相应存取模式的特别表
		例如，我们有一个主表存着用户帐号，其可以被UserID存取
			某查询需要查出某个城市里所有的用户，于是我们可以加入一张表，这张表用城市做主键，所有和这个城市相关的UserID是其Value
		某查询需要查出某个城市里所有的用户，于是我们可以加入一张表，这张表用城市做主键，所有和这个城市相关的UserID是其Value
			无论哪个方式，这都会损伤一些性能，因为需要保持一致性。
	ndex Table 索引表可以被认为是关系型数据库中的视图的等价物。
	适用性
		 BigTable 数据库。
键组合索引 Composite Key Index
	Composite key 键组合是一个很常用的技术，对此，当我们的数据库支持键排序时能得到极大的好处
	Composite key组合键的拼接成为第二排序字段可以让你构建出一种多维索引，
		这很像我们之前说过的 Dimensionality Reduction 降维技术。
	例如，我们需要存取用户统计。
		如果我们需要根据不同的地区来统计用户的分布情况，我们可以把Key设计成这样的格式 (State:City:UserID)
		这样一来，就使得我们可以通过State到City来按组遍历用户，特别是我们的NoSQL数据库支持在key上按区查询
		如：BigTable类的系统
			SELECT Values WHERE state="CA:*"
			SELECT Values WHERE city="CA:San Francisco*"
	适用性
		BigTable 数据库
键组合聚合 Aggregation with Composite Keys
	Composite keys  键组合技术并不仅仅可以用来做索引，同样可以用来区分不用的类型的数据以支持数据分组。
	一个例子，我们有一个海量的日志数组，这个日志记录了互联网上的用户的访问来源
		我们需要计算从某一网站过来的独立访客的数量，在关系型数据库中，我们可能需要下面这样的SQL查询语句
		SELECT count(distinct(user_id)) FROM clicks GROUP BY site
	在NoSQL中
		可以把数据按UserID来排序
			就可以很容易把同一个用户的数据（一个用户并不会产生太多的event）进行处理
				去掉那些重复的站点（使用hash table或是别的什么）
		另一个可选的技术是，我们可以对每一个用户建立一个数据实体，然后把其站点来源追加到这个数据实体中，当然，这样一来，数据的更新在性能相比之下会有一定损失。
	适用性
		Ordered Key-Value Store 排序键值对数据库， BigTable风格的数据库。
反转搜索 Inverted Search – 直接聚合 Direct Aggregation
	这个技术更多的是数据处理技术，而不是数据建模技术
		尽管如此，这个技术还是会影响数据模型
	最主要的想法是使用一个索引来找到满足某条件的数据
		但是把数据聚合起需要使用全文搜索
	例子，我们有很多的日志，其中包括互联网用户和他们的访问来源
		让我们假定每条记录都有一个UserID，还有用户的种类 (Men, Women, Bloggers, 等)，以及用户所在的城市，和访问过的站点
		我们要干的事是，为每个用户种类找到满足某些条件（访问源，所在城市，等）的的独立用户。
	如果我们使用反转搜索，这会让我们把这事干得很容易，如： {Category -> [user IDs]} 或 {Site -> [user IDs]}。
		使用这样的索引， 我们可以取两个或多个UserID要的交集或并集（这个事很容易干，而且可以干得很快，如果这些UserID是排好序的）。
	但是，我们要按用户种类来生成报表会变得有点麻烦，因为我们用语句可能会像下面这样
		SELECT count(distinct(user_id)) ... GROUP BY category
		但这样的SQL很没有效率，因为category数据太多了
		为了应对这个问题，我们可以建立一个直接索引 {UserID -> [Categories]} 然后我们用它来生成报表
	最后，我们需要明白，对每个UserID的随机查询是很没有效率的
		我们可以通过批查询处理来解决这个问题。
		这意味着，对于一些用户集，我们可以进行预处理（不同的查询条件）。
	适用性
		Key-Value Store 键值对数据库， Document Databases文档数据库， BigTable风格的数据库。
```