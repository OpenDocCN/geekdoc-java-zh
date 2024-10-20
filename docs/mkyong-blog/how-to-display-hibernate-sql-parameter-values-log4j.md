# 如何显示 hibernate sql 参数值–Log4j

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/how-to-display-hibernate-sql-parameter-values-log4j/>

## 问题

Hibernate 具有基本的日志记录特性，可以显示带有 [show_sql](http://web.archive.org/web/20220618160333/http://www.mkyong.com/hibernate/hibernate-display-generated-sql-to-console-show_sql-format_sql-and-use_sql_comments/) 配置属性的 SQL 生成语句。

```java
 Hibernate: INSERT INTO mkyong.stock_transaction (CHANGE, CLOSE, DATE, OPEN, STOCK_ID, VOLUME) 
VALUES (?, ?, ?, ?, ?, ?) 
```

然而，这对于调试来说还不够，Hibernate SQL 参数值丢失了。

## 解决方案–Log4j

显示真实的 Hibernate SQL 参数值需要 Log4J。

## 1.在休眠模式下配置 Log4j

按照本文来[在 Hibernate](http://web.archive.org/web/20220618160333/http://www.mkyong.com/hibernate/how-to-configure-log4j-in-hibernate-project/) 中配置 Log4j

## 2.更改日志级别

修改 Log4j 属性文件，在“**Log4j . logger . org . hibernate . type**”属性中将日志级别改为“调试”或“跟踪”。

*文件:log4j.properties*

```java
 # Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

# Root logger option
log4j.rootLogger=INFO, stdout

# Hibernate logging options (INFO only shows startup messages)
log4j.logger.org.hibernate=INFO

# Log JDBC bind parameter runtime arguments
log4j.logger.org.hibernate.type=trace 
```

## 3.完成的

现在显示休眠真实参数值

*输出…*

```java
 Hibernate: INSERT INTO mkyong.stock_transaction (CHANGE, CLOSE, DATE, OPEN, STOCK_ID, VOLUME) 
VALUES (?, ?, ?, ?, ?, ?)
13:33:07,253 DEBUG FloatType:133 - binding '10.0' to parameter: 1
13:33:07,253 DEBUG FloatType:133 - binding '1.1' to parameter: 2
13:33:07,253 DEBUG DateType:133 - binding '30 December 2009' to parameter: 3
13:33:07,269 DEBUG FloatType:133 - binding '1.2' to parameter: 4
13:33:07,269 DEBUG IntegerType:133 - binding '11' to parameter: 5
13:33:07,269 DEBUG LongType:133 - binding '1000000' to parameter: 6 
```

**Note**
If this logging is still not detail enough for you to debug the SQL problem, you can use the P6Spy library to log the **exact SQL statement** that send to database. Check this article – [How to display hibernate sql parameter values with P6Spy](http://web.archive.org/web/20220618160333/http://www.mkyong.com/hibernate/how-to-display-hibernate-sql-parameter-values-solution/)<input type="hidden" id="mkyong-current-postId" value="2673">