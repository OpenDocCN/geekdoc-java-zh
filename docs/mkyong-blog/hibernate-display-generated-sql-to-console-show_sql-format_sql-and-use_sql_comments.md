# 向控制台显示 Hibernate SQL–show _ SQL、format_sql 和 use_sql_comments

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-display-generated-sql-to-console-show_sql-format_sql-and-use_sql_comments/>

Hibernate 有一个内置函数，可以将所有生成的 SQL 语句记录到控制台。您可以通过在 Hibernate 配置文件"`hibernate.cfg.xml`"中添加一个" **show_sql** "属性来启用它。这个函数对于基本的故障排除很有用，并且可以查看 Hibernate 在后面做了什么。

## 1.显示 sql

允许将所有生成的 SQL 语句记录到控制台

```java
 <!--hibernate.cfg.xml -->
<property name="show_sql">true</property> 
```

输出

```java
 Hibernate: insert into mkyong.stock_transaction 
(CHANGE, CLOSE, DATE, OPEN, STOCK_ID, VOLUME) 
values (?, ?, ?, ?, ?, ?) 
```

 ## 2.格式 _sql

格式化生成的 SQL 语句，使其更具可读性，但会占用更多的屏幕空间。:)

```java
 <!--hibernate.cfg.xml -->
<property name="format_sql">true</property> 
```

输出

```java
 Hibernate: 
    insert 
    into
        mkyong.stock_transaction
        (CHANGE, CLOSE, DATE, OPEN, STOCK_ID, VOLUME) 
    values
        (?, ?, ?, ?, ?, ?) 
```

 ## 3.使用 sql 注释

Hibernate 将把注释放在所有生成的 SQL 语句中，以提示生成的 SQL 试图做什么

```java
 <!--hibernate.cfg.xml -->
<property name="use_sql_comments">true</property> 
```

输出

```java
 Hibernate: 
    /* insert com.mkyong.common.StockTransaction
        */ insert 
        into
            mkyong.stock_transaction
            (CHANGE, CLOSE, DATE, OPEN, STOCK_ID, VOLUME) 
        values
            (?, ?, ?, ?, ?, ?) 
```

## 休眠配置文件

`hibernate.cfg.xml`完整的例子。

```java
 <?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <property name="hibernate.bytecode.use_reflection_optimizer">false</property>
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.password">password</property>
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mkyong</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <property name="show_sql">true</property>
        <property name="format_sql">true</property>
        <property name="use_sql_comments">true</property>
    </session-factory>
</hibernate-configuration> 
```

## Hibernate SQL 参数值怎么样？

这个基本的 SQL 日志对于正常的调试来说已经足够好了，但是它不能显示 Hibernate SQL 参数值。需要一些第三方库集成来将 Hibernate SQL 参数值显示到控制台或文件中。检查以下两篇文章:

1.  [如何显示 hibernate sql 参数值–P6Spy](http://web.archive.org/web/20190223085535/http://www.mkyong.com/hibernate/how-to-display-hibernate-sql-parameter-values-solution/)
2.  [如何显示 hibernate sql 参数值–Log4J](http://web.archive.org/web/20190223085535/http://www.mkyong.com/hibernate/how-to-display-hibernate-sql-parameter-values-log4j/)

[hibernate](http://web.archive.org/web/20190223085535/http://www.mkyong.com/tag/hibernate/) [sql](http://web.archive.org/web/20190223085535/http://www.mkyong.com/tag/sql/)







