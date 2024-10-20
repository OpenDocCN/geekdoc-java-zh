# Hibernate 参数绑定示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-parameter-binding-examples/>

如果没有参数绑定，您必须像这样连接参数字符串(错误代码) :

```java
 String hql = "from Stock s where s.stockCode = '" + stockCode + "'";
List result = session.createQuery(hql).list(); 
```

将用户输入中未经检查的值传递给数据库会引发安全问题，因为它很容易被 SQL 注入破解。您必须避免上述糟糕的代码，而是使用参数绑定。

## 休眠参数绑定

参数绑定有两种方式:命名参数或位置参数。

## 1.命名参数

这是最常见和用户友好的方式。它使用冒号后跟参数名(:example)来定义命名参数。查看示例…

##### 示例 1–设置参数

**setParameter** 足够智能，可以为您发现参数数据类型。

```java
 String hql = "from Stock s where s.stockCode = :stockCode";
List result = session.createQuery(hql)
.setParameter("stockCode", "7277")
.list(); 
```

##### 示例 2–设置字符串

你可以用 **setString** 告诉 Hibernate 这个参数日期类型是 String。

```java
 String hql = "from Stock s where s.stockCode = :stockCode";
List result = session.createQuery(hql)
.setString("stockCode", "7277")
.list(); 
```

##### 示例 3–设置属性

这个功能太棒了！您可以将对象传递到参数绑定中。Hibernate 将自动检查对象的属性并与冒号参数匹配。

```java
 Stock stock = new Stock();
stock.setStockCode("7277");
String hql = "from Stock s where s.stockCode = :stockCode";
List result = session.createQuery(hql)
.setProperties(stock)
.list(); 
```

## 2.位置参数

是用问号(？)来定义一个命名参数，并且你必须根据位置序列来设置你的参数。参见示例…

```java
 String hql = "from Stock s where s.stockCode = ? and s.stockName = ?";
List result = session.createQuery(hql)
.setString(0, "7277")
.setParameter(1, "DIALOG")
.list(); 
```

这种方法不支持 **setProperties** 功能。此外，它很容易被破坏，因为绑定参数位置的每次改变都需要改变参数绑定代码。

```java
 String hql = "from Stock s where s.stockName = ? and s.stockCode = ?";
List result = session.createQuery(hql)
.setParameter(0, "DIALOG")
.setString(1, "7277")
.list(); 
```

## 结论

在 Hibernate 参数绑定中，我建议总是使用“**命名参数**，因为它更容易维护，并且编译后的 SQL 语句可以重用(如果绑定参数改变的话)以提高性能。

<input type="hidden" id="mkyong-current-postId" value="3350">