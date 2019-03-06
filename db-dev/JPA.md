# JPA
```md
Java Persistence API
	是 JCP 组织发布的 Java EE 标准之一
		因此任何声称符合 JPA 标准的框架都遵循同样的架构，提供相同的访问API
	是JDK 5.0注解或XML描述对象－关系表的映射关系
		并将运行期的实体对象持久化到数据库中
Why？
	Sun引入新的JPA ORM规范出于两个原因
		简化现有Java EE和Java SE应用开发工作
		Sun希望整合ORM技术，实现天下归一。
```

## Implement
* OpenJPA
```md
 Apache 组织提供的开源项目
	实现了 EJB 3.0 中的 JPA 标准
特性
	封装了和关系型数据库交互的操作
	可以作为独立的持久层框架发挥作用
		也与其它 Java EE 应用框架或者符合 EJB 3.0 标准的容器集成
```
* Hibernate
