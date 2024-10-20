# 如何在 Hibernate 中调用存储过程

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/how-to-call-store-procedure-in-hibernate/>

在本教程中，您将学习如何在 Hibernate 中调用存储过程。

## MySQL 存储过程

这是一个 MySQL 存储过程，它接受股票代码参数并返回相关的股票数据。

```java
 DELIMITER $$

CREATE PROCEDURE `GetStocks`(int_stockcode varchar(20))
BEGIN
   SELECT * FROM stock where stock_code = int_stockcode;
   END $$

DELIMITER ; 
```

在 MySQL 中，你可以用 **call** 关键字简单调用它:

```java
 CALL GetStocks('7277'); 
```

 ## 休眠调用存储过程

在 Hibernate 中，有三种方法可以调用数据库存储过程。

 ## 1.原生 SQL–createsql query

可以使用 **createSQLQuery()** 直接调用存储过程。

```java
 Query query = session.createSQLQuery(
	"CALL GetStocks(:stockCode)")
	.addEntity(Stock.class)
	.setParameter("stockCode", "7277");

List result = query.list();
for(int i=0; i<result.size(); i++){
	Stock stock = (Stock)result.get(i);
	System.out.println(stock.getStockCode());
} 
```

## 2.批注中的 NamedNativeQuery

在 **@NamedNativeQueries** 注释中声明您的存储过程。

```java
 //Stock.java
...
@NamedNativeQueries({
	@NamedNativeQuery(
	name = "callStockStoreProcedure",
	query = "CALL GetStocks(:stockCode)",
	resultClass = Stock.class
	)
})
@Entity
@Table(name = "stock")
public class Stock implements java.io.Serializable {
... 
```

用 **getNamedQuery()** 调用它。

```java
 Query query = session.getNamedQuery("callStockStoreProcedure")
	.setParameter("stockCode", "7277");
List result = query.list();
for(int i=0; i<result.size(); i++){
	Stock stock = (Stock)result.get(i);
	System.out.println(stock.getStockCode());
} 
```

## 3.XML 映射文件中的 sql 查询

在" **sql-query** "标记中声明您的存储过程。

```java
 <!-- Stock.hbm.xml -->
...
<hibernate-mapping>
    <class name="com.mkyong.common.Stock" table="stock" ...>
        <id name="stockId" type="java.lang.Integer">
            <column name="STOCK_ID" />
            <generator class="identity" />
        </id>
        <property name="stockCode" type="string">
            <column name="STOCK_CODE" length="10" not-null="true" unique="true" />
        </property>
        ...
    </class>

    <sql-query name="callStockStoreProcedure">
	<return alias="stock" class="com.mkyong.common.Stock"/>
	<![CDATA[CALL GetStocks(:stockCode)]]>
    </sql-query>

</hibernate-mapping> 
```

用 **getNamedQuery()** 调用它。

```java
 Query query = session.getNamedQuery("callStockStoreProcedure")
	.setParameter("stockCode", "7277");
List result = query.list();
for(int i=0; i<result.size(); i++){
	Stock stock = (Stock)result.get(i);
	System.out.println(stock.getStockCode());
} 
```

## 结论

以上三种方法做的是同样的事情，在数据库中调用一个存储过程。这三种方法没有太大的区别，你选择哪种方法取决于你个人的喜好。

[hibernate](http://web.archive.org/web/20190223081736/http://www.mkyong.com/tag/hibernate/) [stored procedure](http://web.archive.org/web/20190223081736/http://www.mkyong.com/tag/stored-procedure/)







