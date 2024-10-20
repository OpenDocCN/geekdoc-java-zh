# Spring Boot–显示 Hibernate SQL 查询

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-show-hibernate-sql-query/>

在`application.properties`中添加以下行来记录 Hibernate SQL 查询。

application.properties

```java
 #show sql statement
logging.level.org.hibernate.SQL=debug

#show sql values
logging.level.org.hibernate.type.descriptor.sql=trace 
```

## 1.org.hibernate.SQL=debug

application.properties

```java
 logging.level.org.hibernate.SQL=debug 
```

1.1 选择查询。

Console

```java
 2017-02-23 21:36:42 DEBUG org.hibernate.SQL - select customer0_.id as id1_0_, 
	customer0_.created_date as created_date2_0_, customer0_.email as email3_0_, 
	customer0_.name as name4_0_ from customer customer0_ 
```

## 2.org . hibernate . type . descriptor . SQL = trace

application.properties

```java
 logging.level.org.hibernate.SQL=debug
logging.level.org.hibernate.type.descriptor.sql=trace 
```

2.1 选择查询及其值。

Console

```java
 2017-02-23 21:39:23 DEBUG org.hibernate.SQL - select customer0_.id as id1_0_, 
	customer0_.created_date as created_date2_0_, customer0_.email as email3_0_, 
	customer0_.name as name4_0_ from customer customer0_

	2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([id1_0_] : [BIGINT]) - [1]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([created_date2_0_] : [TIMESTAMP]) - [2017-02-11 00:00:00.0]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([email3_0_] : [VARCHAR]) - [111@yahoo.com]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([name4_0_] : [VARCHAR]) - [mkyong]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([id1_0_] : [BIGINT]) - [2]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([created_date2_0_] : [TIMESTAMP]) - [2017-02-12 00:00:00.0]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([email3_0_] : [VARCHAR]) - [222@yahoo.com]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([name4_0_] : [VARCHAR]) - [yflow]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([id1_0_] : [BIGINT]) - [3]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([created_date2_0_] : [TIMESTAMP]) - [2017-02-13 00:00:00.0]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([email3_0_] : [VARCHAR]) - [333@yahoo.com]
2017-02-23 21:39:23 TRACE o.h.t.descriptor.sql.BasicExtractor - extracted value ([name4_0_] : [VARCHAR]) - [zilap] 
```

2.2 插入查询及其值。

Console

```java
 2017-02-23 21:44:15 DEBUG org.hibernate.SQL - select customer_seq.nextval from dual
2017-02-23 21:44:15 DEBUG org.hibernate.SQL - insert into customer (created_date, email, name, id) values (?, ?, ?, ?)

2017-02-23 21:44:15 TRACE o.h.type.descriptor.sql.BasicBinder - binding parameter [1] as [TIMESTAMP] - [Thu Feb 23 21:44:15 SGT 2017]
2017-02-23 21:44:15 TRACE o.h.type.descriptor.sql.BasicBinder - binding parameter [2] as [VARCHAR] - [aa]
2017-02-23 21:44:15 TRACE o.h.type.descriptor.sql.BasicBinder - binding parameter [3] as [VARCHAR] - [a]
2017-02-23 21:44:15 TRACE o.h.type.descriptor.sql.BasicBinder - binding parameter [4] as [BIGINT] - [1] 
```

**Note**
You might be interested in this article – [Hibernate Logging Guide](http://web.archive.org/web/20220930231922/http://docs.jboss.org/hibernate/orm/current/topical/html_single/logging/Logging.html)

## 参考

1.  [Spring Boot SLF4J 测井示例](http://web.archive.org/web/20220930231922/https://www.mkyong.com/spring-boot/spring-boot-slf4j-logging-example/)

<input type="hidden" id="mkyong-current-postId" value="14463">