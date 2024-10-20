# Hibernate 本地 SQL 查询示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-native-sql-queries-examples/>

在 Hibernate 中，HQL 或标准查询应该能够让你执行几乎任何你想要的 SQL 查询。然而，许多开发人员抱怨 Hibernate 生成的 SQL 语句太慢，更喜欢生成自己的 SQL (native SQL)语句。

## 本地 SQL 查询示例

Hibernate 提供了一个 **createSQLQuery** 方法，让您可以直接调用本地 SQL 语句。

1.在本例中，您告诉 Hibernate 返回一个 Stock.class，所有 select 数据(*)将自动匹配您的 Stock.class 属性。

```java
 Query query = session.createSQLQuery(
"select * from stock s where s.stock_code = :stockCode")
.addEntity(Stock.class)
.setParameter("stockCode", "7277");
List result = query.list(); 
```

2.在这个例子中，Hibernate 将返回一个对象数组。

```java
 Query query = session.createSQLQuery(
"select s.stock_code from stock s where s.stock_code = :stockCode")
.setParameter("stockCode", "7277");
List result = query.list(); 
```

或者，您也可以使用**命名查询**来调用您的本地 SQL 语句。这里的见 [Hibernate 命名查询示例。](http://web.archive.org/web/20190309024532/http://www.mkyong.com/hibernate/hibernate-named-query-examples/)

 ## Hibernate 生成的 SQL 语句很慢！？

我不同意“Hibernate 生成的 SQL 语句很慢”的说法。很多时候，我发现这是因为开发人员没有很好地理解表关系，并且做了一些错误的表映射或者误用了获取策略。这将导致 Hibernate 生成一些不必要的 SQL 语句或加入一些不必要的表…而开发人员喜欢以此为借口，创建自己的 SQL 语句来快速修复 bug，而没有意识到核心问题将导致更多的 bug 等待。

 ## 结论

我承认有时候原生 SQL 语句确实比 HQL 更方便、更容易，但是，请仔细想想为什么需要原生 SQL 语句？这真的是冬眠做不到吗？如果是，那就去吧~

*P.S 在 Oracle 数据库中，我更喜欢在许多关键性能查询中使用原生 SQL 语句，因为我需要放“提示”来提高 Oracle 查询性能。*

[hibernate](http://web.archive.org/web/20190309024532/http://www.mkyong.com/tag/hibernate/)







