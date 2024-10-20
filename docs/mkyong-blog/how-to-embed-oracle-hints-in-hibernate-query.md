# 如何在 Hibernate 查询中嵌入 Oracle 提示

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/how-to-embed-oracle-hints-in-hibernate-query/>

使用 Oracle 提示，您可以改变 Oracle 执行计划，以影响 Oracle 从数据库中检索数据的方式。请点击此处了解有关 [Oracle 优化器提示]( http://download.oracle.com/docs/cd/B10501_01/server.920/a96533/hintsref.htm)的更多详细信息。

在 Hibernate 中，有可能将 Oracle 提示嵌入 Hibernate 查询中吗？

## Hibernate setComment()？

能否用 Hibernate 自定义注释" **setComment()** "函数将 Oracle 提示嵌入到 HQL 中？让我们来看一个例子

## 1.原始 Hibernate 查询

这是一个简单的选择 HQL 检索股票代码的股票。

```java
 String hql = "from Stock s where s.stockCode = :stockCode";
List result = session.createQuery(hql)
.setString("stockCode", "7277")
.list(); 
```

输出

```java
 Hibernate: 
    select
        stock0_.STOCK_ID as STOCK1_0_,
        stock0_.STOCK_CODE as STOCK2_0_,
        stock0_.STOCK_NAME as STOCK3_0_ 
    from mkyong.stock stock0_ 
    where stock0_.STOCK_CODE=? 
```

## 2.尝试 Hibernate setComment()

在 hibernate 的配置文件(hibernate.cfg.xml)中启用**Hibernate . use _ SQL _ comments**，以便将自定义注释输出到日志文件或控制台。

```java
 <!-- hibernate.cfg.xml -->
<?xml version="1.0" encoding="utf-8"?>
...
<hibernate-configuration>
 <session-factory>
    ...
    <property name="show_sql">true</property>
    <property name="format_sql">true</property>
    <property name="use_sql_comments">true</property>
    <mapping class="com.mkyong.common.Stock" />
  </session-factory>
</hibernate-configuration> 
```

使用 Hibernate **setComment()** 向查询中插入自定义注释。

```java
 String hql = "from Stock s where s.stockCode = :stockCode";
List result = session.createQuery(hql)
.setString("stockCode", "7277")
.setComment("+ INDEX(stock idx_stock_code)")
.list(); 
```

输出

```java
 Hibernate: 
    /* + INDEX(stock idx_stock_code) */ select
        stock0_.STOCK_ID as STOCK1_0_,
        stock0_.STOCK_CODE as STOCK2_0_,
        stock0_.STOCK_NAME as STOCK3_0_ 
    from mkyong.stock stock0_ 
    where stock0_.STOCK_CODE=? 
```

## 3.这是工作吗？

事实并非如此，Hibernate 自定义注释有两个问题。

1.Oracle 提示必须追加在“select”之后，而不是之前。

Hibernate 生成的查询

```java
 /* + INDEX(stock idx_stock_code) */ select 
```

正确的做法应该是…

```java
 select  /*+ INDEX(stock idx_stock_code) */ 
```

2.Hibernate 会自动在“/* +”之间添加一个额外的空格。

在 Hibernate 中，仍然没有官方方法将 Oracle 提示嵌入到 Hibernate 查询语言(HQL)中。

附注:感谢皮特对此的贡献。

## 工作液

唯一的解决方案是使用 Hibernate **createSQLQuery** 方法来执行原生 SQL 语句。

```java
 String hql = "/*+ INDEX(stock idx_stock_code) */ 
    select * from stock s where s.stock_code = :stockCode";
List result = session.createQuery(hql)
.setString("stockCode", "7277")
.list(); 
```

输出

```java
 Hibernate: 
    /*+ INDEX(stock idx_stock_code) */ select * 
    from stock s where s.stock_code = ? 
```

更多[原生 SQL 查询示例](http://web.archive.org/web/20220906170456/http://www.mkyong.com/hibernate/hibernate-native-sql-queries-examples/)。

<input type="hidden" id="mkyong-current-postId" value="3352">