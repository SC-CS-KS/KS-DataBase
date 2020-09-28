# Hbase vs. Redis

* 读写性能

HBase写快读慢，HBase的读取时长通常是几毫秒，而Redis的读取时长通常是几十微秒。性能相差非常大。

* 数据类型

HBase 和Redis 都支持KV类型。但是 Redis支持 List、Set等更丰富的类型。

* 数据量

Redis支持的数据量通常受内存限制，而HBase没有这个限制，可以存储远超内存大小的数据。

* 部署难易

HBase 部署需要依赖 Hadoop、Zookeeper等服务，而 Redis的部署非常简单。

* 数据可靠性

HBase采用 WAL，先记录日志再写入数据，理论上不会丢失数据。
而Redis采用的是异步复制数据，在 failover 时可能会丢失数据。

* 应用场景

HBase适合做大数据的持久存储，而Redis比较适合做缓存。
如果数据丢失是不能容忍的，那就用只能用HBase；
如果需要一个高性能的环境，而且能够容忍一定的数据丢失，那完全可以考虑使用Redis。

* 两者的结合

HBase可以用来做数据的固化，也就是数据存储，做这个他非常合适。Redis 适合做 cache。
可以用HBase+Redis实现数据仓库加缓存数据库，速度和扩展性都兼顾。
