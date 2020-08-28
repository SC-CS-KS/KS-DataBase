# DataType

* 通用数据类型

字符型
	CHAR
	VARCHAR
文本型
	Text
数值型
	INT
	NUMERIC
	MONEY
逻辑型
	BIT
日期型
	DATETIME


* NULL 值

如果表中的某个列是可选的，那么我们可以在不向该列添加值的情况下插入新记录或更新已有的记录。
		这意味着该字段将以 NULL 值保存。
	NULL 值的处理方式与其他值不同。
		NULL 用作未知的或不适用的值的占位符。
注意
	无法比较 NULL 和 0；它们是不等价的。
		无法使用比较运算符来测试 NULL 值，比如 =、< 或 <>。
运算符
	IS NULL
		SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NULL
	IS NOT NULL
NULL 函数
	ISNULL()
	NVL()
	IFNULL()
	COALESCE()
