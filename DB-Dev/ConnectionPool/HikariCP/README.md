# [HikariCP](https://github.com/brettwooldridge/HikariCP)

```md
	Hikari
		Hi·ka·ri [hi·ka·'lē] (Origin: Japanese): light; ray
	BoneCP
		BoneCP是一个快速，开源的数据库连接池
			但BoneCP这个连接池在2013年停止更新了
				从BoneCP到HikariCP
				就是为了让步于HikariCP这个连接池。
	HikariCP同样是一个十分快速、简单、可靠的及十分轻量级的连接池
		只有130KB，在GitHub上看到的是"光HikariCP"的名称，光就是说明它十分快
		性能真是十分强悍
			简直就是碾压其他各种连接池
优势
	字节码精简
		优化代码，直到编译后的字节码最少，这样，CPU缓存可以加载更多的程序代码
	优化代理和拦截器
		减少代码，例如HikariCP的Statement proxy只有100行代码，只有BoneCP的十分之一
	自定义数组类型（FastList）代替ArrayList
		避免每次get()调用都要进行range check，避免调用remove()时的从头到尾的扫描
	自定义集合类型（ConcurrentBag）
		提高并发读写的效率
	其他针对BoneCP缺陷的优化
		比如对于耗时超过一个CPU时间片的方法调用的研究
```

## Resources
* [HikariCP源码解读](https://www.cnblogs.com/taisenki/tag/Hikaricp/)
