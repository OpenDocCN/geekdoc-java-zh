# 休眠标准示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-criteria-examples/>

Hibernate Criteria API 是 Hibernate 查询语言(HQL)的一个更加面向对象和优雅的替代品。对于有许多可选搜索标准的应用程序来说，这总是一个好的解决方案。

## HQL 的例子和标准

这里有一个案例研究，使用可选的搜索标准(开始日期、结束日期和数量、按日期排序)来检索一个 **StockDailyRecord** 的列表。

## 1.HQL 的例子

在 HQL，您需要比较这是否是附加“where”语法的第一个标准，并将日期格式化为合适的格式。它的工作，但长代码是丑陋的，笨重的和容易出错的字符串连接可能会引起安全问题，如 SQL 注入。

```java
 public static List getStockDailtRecord(Date startDate,Date endDate,
   Long volume,Session session){

   SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
   boolean isFirst = true; 

   StringBuilder query = new StringBuilder("from StockDailyRecord ");

   if(startDate!=null){
	if(isFirst){
		query.append(" where date >= '" + sdf.format(startDate) + "'");
	}else{
		query.append(" and date >= '" + sdf.format(startDate) + "'");
	}
	isFirst = false;
   }

   if(endDate!=null){
	if(isFirst){
		query.append(" where date <= '" + sdf.format(endDate) + "'");
	}else{
		query.append(" and date <= '" + sdf.format(endDate) + "'");
	}
	isFirst = false;
   }

   if(volume!=null){
	if(isFirst){
		query.append(" where volume >= " + volume);
	}else{
		query.append(" and volume >= " + volume);
	}
	isFirst = false;
   }

   query.append(" order by date");
   Query result = session.createQuery(query.toString());

   return result.list();
} 
```

## 2.标准示例

在条件中，您不需要比较这是否是附加“where”语法的第一个条件，也不需要设置日期格式。代码行减少了，一切都以更优雅和面向对象方式处理。

```java
 public static List getStockDailyRecordCriteria(Date startDate,Date endDate,
        Long volume,Session session){

	Criteria criteria = session.createCriteria(StockDailyRecord.class);
	if(startDate!=null){
		criteria.add(Expression.ge("date",startDate));
	}
	if(endDate!=null){
		criteria.add(Expression.le("date",endDate));
	}
	if(volume!=null){
		criteria.add(Expression.ge("volume",volume));
	}
	criteria.addOrder(Order.asc("date"));

	return criteria.list();
  } 
```

## 标准 API

让我们来看看一些流行的标准 API 函数。

## 1.标准基本查询

创建一个 criteria 对象，并从数据库中检索所有“StockDailyRecord”记录。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class); 
```

## 2.标准排序查询

结果按“日期”升序排序。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
    .addOrder( Order.asc("date") ); 
```

结果按“日期”降序排序。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
    .addOrder( Order.desc("date") ); 
```

## 3.标准限制查询

Restrictions 类提供了许多方法来进行比较操作。

##### 限制. eq

确保值等于 10000。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
    .add(Restrictions.eq("volume", 10000)); 
```

##### 限制。lt，le，gt，ge

确保音量小于 10000。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
   .add(Restrictions.lt("volume", 10000)); 
```

请确保音量小于或等于 10000。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
   .add(Restrictions.le("volume", 10000)); 
```

确保音量大于 10000。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
   .add(Restrictions.gt("volume", 10000)); 
```

请确保音量大于或等于 10000。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
   .add(Restrictions.ge("volume", 10000)); 
```

##### 限制，比如

确保股票名称以“MKYONG”开头，后跟任意字符。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
   .add(Restrictions.like("stockName", "MKYONG%")); 
```

##### 限制。介于

请确保该日期在开始日期和结束日期之间。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
   .add(Restrictions.between("date", startDate, endDate)); 
```

##### Restrictions.isNull，isNotNull

请确保卷为空。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
   .add(Restrictions.isNull("volume")); 
```

请确保卷不为空。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class)
   .add(Restrictions.isNotNull("volume")); 
```

许多其他的限制函数可以在这里找到。
[https://www . hibernate . org/Hib _ docs/v3/API/org/hibernate/criterion/restrictions . html](http://web.archive.org/web/20221023054403/https://www.hibernate.org/hib_docs/v3/api/org/hibernate/criterion/Restrictions.html)

## 3.结果分页标准

Criteria 提供了很少的函数来使分页变得非常容易。从第 20 条记录开始，从数据库中检索接下来的 10 条记录。

```java
 Criteria criteria = session.createCriteria(StockDailyRecord.class);
criteria.setMaxResults(10);
criteria.setFirstResult(20); 
```

## 为什么不是标准！？

标准 API 确实带来了一些缺点。

## 1.性能问题

您无法控制 Hibernate 生成的 SQL 查询，如果生成的查询很慢，您很难调优查询，并且您的数据库管理员可能不喜欢它。

## 1.维护问题

所有的 SQL 查询都分散在 Java 代码中，当一个查询出错时，您可能要花时间在您的应用程序中查找问题查询。另一方面，存储在 Hibernate 映射文件中的命名查询更容易维护。

## 结论

没有什么是完美的，请考虑您的项目需求并明智地使用它。

<input type="hidden" id="mkyong-current-postId" value="3346">