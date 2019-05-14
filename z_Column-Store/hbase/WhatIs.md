# What is Hbase
```md
Apache HBase是一个开源的，分布式的，版本化的非关系数据库，
模仿 Google 的 Bigtable 的结构化数据分布式存储系统。

正如 Bigtable 利用 Google文件系统提供的分布式数据存储一样，
Apache HBase 在 Hadoop 和 HDFS之上提供类似 Bigtable 的功能。
```

## Features
```md
线性和模块化可扩展性。
严格一致的读写操作。
表的自动和可配置分片
RegionServers 之间的自动故障转移支持。
方便的基类，用于使用Apache HBase 表支持 Hadoop MapReduce 作业。
易于使用的Java API，用于客户端访问。
阻止缓存和布隆过滤器以进行实时查询。
查询谓词通过服务器端过滤器下推
Thrift 网关和 REST-ful Web服务，支持XML，Protobuf 和二进制数据编码选项
可扩展的基于jruby（JIRB）的外壳
支持通过 Hadoop 指标子系统将指标导出到文件或 Ganglia 或通过 JMX
```