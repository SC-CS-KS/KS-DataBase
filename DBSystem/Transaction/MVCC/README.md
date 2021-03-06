# MVCC (Multi-Version Concurrency Control)

是一种并发控制的方法，一般在数据库管理系统中，实现对数据库的并发访问，在编程语言中实现事务内存
在MVCC之前，主要采用Two-phase commit，但Two-phase commit过程中需要对读写操作进行加锁，
这会带来显著的性能问题，因此，Two-phase commit在读写高并发场景中已被逐步摒弃。

每个连接到数据库的读者，在某个瞬间看到的是数据库的一个快照，
写者写操作造成的变化在写操作完成之前（或者数据库事务提交之前）对于其他的读者来说是不可见的。
当一个 MVCC 数据库需要更一个一条数据记录的时候，它不会直接用新数据覆盖旧数据，
而是将旧数据标记为过时（obsolete）并在别处增加新版本的数据，这样就会有存储多个版本的数据，但是只有一个是最新的。

这种方式允许读者读取在他读之前已经存在的数据，即使这些在读的过程中半路被别人修改、删除了，也对先前正在读的用户没有影响。
这种多版本的方式避免了填充删除操作在内存和磁盘存储结构造成的空洞的开销，
但是需要系统周期性整理（sweep through）以真实删除老的、过时的数据。
	
MVCC 提供了时点（point in time）一致性视图
MVCC 并发控制下的读事务一般使用时间戳或者事务 ID去标记当前读的数据库的状态（版本），读取这个版本的数据。
读、写事务相互隔离，不需要加锁。
读写并存的时候，写操作会根据目前数据库的状态，创建一个新版本，并发的读则依旧访问旧版本的数据。

## 场景

对于面向文档的数据库（Document-oriented database，也即半结构化数据库）来说
这种方式允许系统将整个文档写到磁盘的一块连续区域上，需要更新的时候，直接重写一个版本，
而不是对文档的某些比特位、分片切除，或者维护一个链式的、非连续的数据库结构。

## 缺点

存储多个版本数据的冗余开销

## 优点

读操作永不会被阻塞，这对那些以读操作为主的数据库来说非常重要

快照隔离（snapshot isolation），主要解决Two-phase commit带来的性能底下的问题
		
每一个数据都多个版本，读写能够并发进行
每个事务相当于看到数据的一个快照，写写不能并发，写写操作时需要加上行锁。
谁先获取到行锁，谁可以顺利执行(采用“first win”的原则)，后面的写事务要么abort。
要么等待前面的事务执行完成后再执行（以Oracle/SQL Server/MySQL等数据库为代表）
