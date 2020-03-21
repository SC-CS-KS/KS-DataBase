# 

```md
SQL 约束用于规定表中的数据规则。
	如果存在违反约束的数据行为，行为会被约束终止。
约束可以在创建表时规定（通过 CREATE TABLE 语句）
	或者在表创建之后规定（通过 ALTER TABLE 语句）。
完整性约束
	完整性约束用于保证关系型数据库中数据的精确性和一致性。
		完整性约束是为了表的数据的正确性
			如果数据不正确，那么一开始就不能添加到表中。
		保证授权用户对数据库所做的修改不会破坏数据的一致性。
	对于关系型数据库来说，数据完整性由参照完整性（referential integrity，RI）来保证。
```

## 类型
```md
NOT NULL 
	默认情况下，一列可以容纳NULL值
		如果不想列有NULL值，那么需要不允许此列指定NULL定义这样的约束。
	一个NULL和没有数据是不一样的，相反它代表了未知的数据。
UNIQUE
	唯一约束防止在特定的列有相同的两个纪录值
	持命名的多个列的约束
		ALTER TABLE CUSTOMERS ADD CONSTRAINT myUniqueConstraint UNIQUE(AGE, SALARY);
	删除UNIQUE约束
		ALTER TABLE CUSTOMERS DROP CONSTRAINT myUniqueConstraint;
	使用MySQL，那么可以使用下面的语法：
		ALTER TABLE CUSTOMERS DROP INDEX myUniqueConstraint;
PRIMARY KEY 
	主键是一个在一个数据库表中唯一标识每个行/记录表中的一个字段
		主键必须包含唯一值。主键列不能有NULL值。
	一个表只能有一个主键，它可以由单个或多个字段组成
		当多个字段被用来作为主键，它们被称为复合键。
	ALTER TABLE CUSTOMERS  ADD CONSTRAINT PK_CUSTID PRIMARY KEY (ID, NAME);
	ALTER TABLE CUSTOMERS DROP PRIMARY KEY ;
	注意
		如果您使用ALTER TABLE语句添加主键
			主键列必须已经被声明为不包含NULL值
FOREIGN KEY
	保证一个表中的数据匹配另一个表中的值的参照完整性。
	外键是用于两个表链接在一起的键
		有时被称为一个参考项。
	外键是一列或多列的值匹配在不同的表的主键的组合。
CHECK
	CHECK 约束用于限制列中的值的范围。
		如果对单个列定义 CHECK 约束，那么该列只允许特定的值。
		如果对一个表定义 CHECK 约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。
DEFAULT
	规定没有给列赋值时的默认值。
```