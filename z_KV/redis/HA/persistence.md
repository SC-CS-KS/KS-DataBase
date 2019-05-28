# [Redis Persistence](http://doc.redisfans.com/topic/persistence.html)

## 持久化方式
* RDB 
```md
持久化可以在指定的时间间隔内生成数据集的时间点快照（point-in-time snapshot）。
```
* AOF
```md
持久化记录服务器执行的所有写操作命令，并在服务器启动时，通过重新执行这些命令来还原数据集。 
AOF 文件中的命令全部以 Redis 协议的格式来保存，新命令会被追加到文件的末尾。 
Redis 还可以在后台对 AOF 文件进行重写（rewrite），使得 AOF 文件的体积不会超出保存数据集状态所需的实际大小。
```
* 同时使用 AOF 持久化和 RDB 持久化
```md
 在这种情况下， 当 Redis 重启时， 它会优先使用 AOF 文件来还原数据集， 
 因为 AOF 文件保存的数据集通常比 RDB 文件所保存的数据集更完整。
```
* 关闭持久化功能

### RDB
* 配置文件
```md
save 900 1 // 900秒内，有1条写入，则产生快照 
save 300 1000 // 如果300秒内有1000次写入，则产生快照
save 60 10000 // 如果60秒内有10000次写入，则产生快照
(3个选项都屏蔽，则rdb禁用)

rdbcompression yes // 导出的rdb文件是否压缩
Rdbchecksum yes // 导入rbd恢复时数据时,要不要检验rdb的完整性
dbfilename dump.rdb //导出来的rdb文件名
dir ./ //rdb的放置路径
```

### AOF
* 配置文件
```md
appendonly no # 是否打开 AOF 日志功能
appendfsync always # 每1个命令，都立即同步到AOF，安全，速度慢
appendfsync everysec # 折衷方案，每秒写1次。
appendfsync no # 写入工作交给操作系统，由操作系统判断缓冲区大小，统一写入到aof。同步频率低，速度快。

no-appendfsync-on-rewrite yes: # 正在导出rdb快照的过程中，要不要停止同步AOF。
auto-aof-rewrite-percentage 100 #AOF文件大小比起上次重写时的大小，增长率100%时，重写
auto-aof-rewrite-min-size 64mb #AOF文件，至少超过64M时，重写。
```


