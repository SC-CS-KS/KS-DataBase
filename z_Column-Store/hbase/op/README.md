# HBase OP


## Install
```sh
axel -a -n 10 https://mirrors.tuna.tsinghua.edu.cn/apache/hbase/2.0.5/hbase-2.0.5-bin.tar.gz
```
### Config
* hbase-env.sh
```md
 export JAVA_HOME=/usr/lib/jvm/java-1.8.0
 export HBASE_MANAGES_ZK=false //默认值是true，它表示HBase使用自身自带的 Zookeeper 实例。只能为单机或伪分布模式下的HBase提供服务。
```
* hbase-site.xml
### Star
```sh
# bin/hbase-daemon.sh start zookeeper
# bin/hbase-daemon.sh start master
# bin/hbase-daemon.sh start regionserver
```
```sh
# jps
2975 HQuorumPeer
3302 HRegionServer
3051 HMaster
```
### Test
```sh
$ ./bin/hbase shell
hbase(main):001:0>
```
* 列出有多少个表空间
```md
hbase(main):001:0> list_namespace
NAMESPACE
default
hbase
2 row(s)

hbase默认有两个表空间，它们是default和hbase
```
* 查看default表空间下有哪些表:
```md
hbase(main):003:0> list_namespace_tables 'hbase'
TABLE
meta
namespace
2 row(s)
```

* 创建表空
```md
hbase(main):004:0> create_namespace 'sunny_test'
Took 0.2940 seconds
```
* 查看表空间信息：
```md
hbase(main):007:0> describe_namespace 'sunny_test'
DESCRIPTION
{NAME => 'sunny_test'}
Took 0.0162 seconds
```
* 创建表
```md
hbase(main):008:0> create 'sunny_test:tuser',{NAME=>'baseinfo',VERSIONS=>5},{NAME=>'extrainfo',VERSIONS=>3}
Created table sunny_test:tuser
Took 1.3557 seconds
=> Hbase::Table - sunny_test:tuser
```
