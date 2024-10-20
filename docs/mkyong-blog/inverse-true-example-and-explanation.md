# inverse = "true "示例和说明

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/inverse-true-example-and-explanation/>

**Always put inverse=”true” in your collection variable ?**
There are many Hibernate articles try to explain the “inverse” with many Hibernate “official” jargon, which is very hard to understand (at least to me). In few articles, they even suggested that just forget about what is “inverse”, and always put **inverse=”true”** in the collection variable.

这句话永远是正确的——“在集合变量中放置逆=真”,但是不要对此视而不见，试着理解背后的原因对于优化你的 Hibernate 性能是至关重要的。

## 什么是“逆”？

这是 Hibernate 中最容易混淆的关键词，至少我花了相当长的时间才理解。**逆**关键字在**一对多**和**多对多**关系中总是声明的(多对一没有逆关键字)，表示哪一方负责照顾关系。

## “逆”，应该改成“关系主”？

在 Hibernate 中，只有“关系所有者”应该维护关系，创建“反向”关键字来定义哪一方是维护关系的所有者。然而“反向”关键字本身不够详细，我建议将关键字改为“ **relationship_owner** ”。

简而言之，inverse="true "表示这是关系所有者，inverse="false "(默认)表示不是。

## 1.一对多关系

这是一个**一对多**的关系表设计，一个股票表在 STOCK_DAILY_RECORD 表中有多次出现。



![one to many relationship](img/90097f5275bce534a9c307dc792d237c.png "one-to-many-relationship-inverse")

## 2.Hibernate 实现

请参见 XML 映射文件中的 Hibernate 实现。

*文件:Stock.java*

```java
 public class Stock implements java.io.Serializable {
   ...
   private Set<StockDailyRecord> stockDailyRecords = 
						new HashSet<StockDailyRecord>(0);
   ... 
```

*文件:StockDailyRecord.java*

```java
 public class StockDailyRecord implements java.io.Serializable {
   ...
   private Stock stock;
   ... 
```

*文件:Stock.hbm.xml*

```java
 <hibernate-mapping>
    <class name="com.mkyong.common.Stock" table="stock" ...>
    ...
    <set name="stockDailyRecords" table="stock_daily_record" fetch="select">
        <key>
            <column name="STOCK_ID" not-null="true" />
        </key>
        <one-to-many class="com.mkyong.common.StockDailyRecord" />
    </set>
    ... 
```

*文件:StockDailyRecord.hbm.xml*

```java
 <hibernate-mapping>
  <class name="com.mkyong.common.StockDailyRecord" table="stock_daily_record" ...>
  ...
  <many-to-one name="stock" class="com.mkyong.common.Stock">
       <column name="STOCK_ID" not-null="true" />
  </many-to-one>
  ... 
```

## 3.逆=真/假

Inverse 关键字适用于一对多关系。这里的问题是，如果在“股票”对象中执行保存或更新操作，它是否应该更新“股票每日记录”关系？

*文件:Stock.hbm.xml*

```java
 <class name="com.mkyong.common.Stock" table="stock" ...>
    ...
    <set name="stockDailyRecords" table="stock_daily_record" inverse="{true/false}" fetch="select">
        <key>
            <column name="STOCK_ID" not-null="true" />
        </key>
        <one-to-many class="com.mkyong.common.StockDailyRecord" />
    </set>
    ... 
```

**1。inverse="true"**

如果 set 变量中的 inverse="true "，则表示" stock_daily_record "是关系所有者，因此 stock 不会更新关系。

```java
 <class name="com.mkyong.common.Stock" table="stock" ...>
    ...
	<set name="stockDailyRecords" table="stock_daily_record" inverse="true" > 
```

**2。inverse="false"**

如果 set 变量中的 inverse="false "(默认值)，则表示“股票”是关系所有者，股票将更新关系。

```java
 <class name="com.mkyong.common.Stock" table="stock" ...>
	...
	<set name="stockDailyRecords" table="stock_daily_record" inverse="false" > 
```

查看以下更多示例:

## 4.inverse="false "示例

如果没有定义关键字“inverse ”,将使用 inverse = "false ",即

```java
 <!--Stock.hbm.xml-->
<class name="com.mkyong.common.Stock" table="stock" ...>
	...
	<set name="stockDailyRecords" table="stock_daily_record" inverse="false"> 
```

这意味着“股票”是关系的所有者，它将维持关系。

**插入示例…**

当保存一个“股票”对象时，Hibernate 将生成三个 SQL 语句，两个插入和一个更新。

```java
 session.beginTransaction();

    Stock stock = new Stock();
    stock.setStockCode("7052");
    stock.setStockName("PADINI");

    StockDailyRecord stockDailyRecords = new StockDailyRecord();
    stockDailyRecords.setPriceOpen(new Float("1.2"));
    stockDailyRecords.setPriceClose(new Float("1.1"));
    stockDailyRecords.setPriceChange(new Float("10.0"));
    stockDailyRecords.setVolume(3000000L);
    stockDailyRecords.setDate(new Date());

    stockDailyRecords.setStock(stock);        
    stock.getStockDailyRecords().add(stockDailyRecords);

    session.save(stock);
    session.save(stockDailyRecords);

    session.getTransaction().commit(); 
```

*输出…*

```java
 Hibernate: 
    insert 
    into
        mkyongdb.stock
        (STOCK_CODE, STOCK_NAME) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        mkyongdb.stock_daily_record
        (STOCK_ID, PRICE_OPEN, PRICE_CLOSE, PRICE_CHANGE, VOLUME, DATE) 
    values
        (?, ?, ?, ?, ?, ?)
Hibernate: 
    update
        mkyongdb.stock_daily_record 
    set
        STOCK_ID=? 
    where
        RECORD_ID=? 
```

股票将更新"**股票 _ 日报 _ 记录。STOCK_ID** "通过设置变量(stockDailyRecords)，因为 STOCK 是关系所有者。

**Note**
The third statement is really NOT necessary.

**更新示例…**

当一个“股票”对象被更新时，Hibernate 将生成两条 SQL 语句，一条插入，一条更新。

```java
 session.beginTransaction();

    Stock stock = (Stock)session.get(Stock.class, 57);

    StockDailyRecord stockDailyRecords = new StockDailyRecord();
    stockDailyRecords.setPriceOpen(new Float("1.2"));
    stockDailyRecords.setPriceClose(new Float("1.1"));
    stockDailyRecords.setPriceChange(new Float("10.0"));
    stockDailyRecords.setVolume(3000000L);
    stockDailyRecords.setDate(new Date());

    stockDailyRecords.setStock(stock);        
    stock.getStockDailyRecords().add(stockDailyRecords);

    session.save(stockDailyRecords);
    session.update(stock);

    session.getTransaction().commit(); 
```

*输出…*

```java
 Hibernate: 
    insert 
    into
        mkyongdb.stock_daily_record
        (STOCK_ID, PRICE_OPEN, PRICE_CLOSE, PRICE_CHANGE, VOLUME, DATE) 
    values
        (?, ?, ?, ?, ?, ?)
Hibernate: 
    update
        mkyongdb.stock_daily_record 
    set
        STOCK_ID=? 
    where
        RECORD_ID=? 
```

**Note**
Again, the third statement is NOT necessary.

## 5.inverse="true "示例

如果定义了关键字“逆=真”:

```java
 <!--Stock.hbm.xml-->
<class name="com.mkyong.common.Stock" table="stock" ...>
	...
	<set name="stockDailyRecords" table="stock_daily_record" inverse="true"> 
```

现在，它的意思是" **stockDailyRecords** "是关系所有者，而" stock "不会维护关系。

**插入示例…**

当保存一个“股票”对象时，Hibernate 会生成两条 SQL insert 语句。

```java
 session.beginTransaction();

    Stock stock = new Stock();
    stock.setStockCode("7052");
    stock.setStockName("PADINI");

    StockDailyRecord stockDailyRecords = new StockDailyRecord();
    stockDailyRecords.setPriceOpen(new Float("1.2"));
    stockDailyRecords.setPriceClose(new Float("1.1"));
    stockDailyRecords.setPriceChange(new Float("10.0"));
    stockDailyRecords.setVolume(3000000L);
    stockDailyRecords.setDate(new Date());

    stockDailyRecords.setStock(stock);        
    stock.getStockDailyRecords().add(stockDailyRecords);

    session.save(stock);
    session.save(stockDailyRecords);

    session.getTransaction().commit(); 
```

*输出…*

```java
 Hibernate: 
    insert 
    into
        mkyongdb.stock
        (STOCK_CODE, STOCK_NAME) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        mkyongdb.stock_daily_record
        (STOCK_ID, PRICE_OPEN, PRICE_CLOSE, PRICE_CHANGE, VOLUME, DATE) 
    values
        (?, ?, ?, ?, ?, ?) 
```

**更新示例…**

当一个“股票”对象被更新时，Hibernate 将生成一条 SQL 语句。

```java
 session.beginTransaction();

    Stock stock = (Stock)session.get(Stock.class, 57);

    StockDailyRecord stockDailyRecords = new StockDailyRecord();
    stockDailyRecords.setPriceOpen(new Float("1.2"));
    stockDailyRecords.setPriceClose(new Float("1.1"));
    stockDailyRecords.setPriceChange(new Float("10.0"));
    stockDailyRecords.setVolume(3000000L);
    stockDailyRecords.setDate(new Date());

    stockDailyRecords.setStock(stock);        
    stock.getStockDailyRecords().add(stockDailyRecords);

    session.save(stockDailyRecords);
    session.update(stock);

    session.getTransaction().commit(); 
```

*输出…*

```java
 Hibernate: 
    insert 
    into
        mkyongdb.stock_daily_record
        (STOCK_ID, PRICE_OPEN, PRICE_CLOSE, PRICE_CHANGE, VOLUME, DATE) 
    values
        (?, ?, ?, ?, ?, ?) 
```

**inverse vs cascade**
Many people like to compare between inverse and cascade, but both are totally different notions, see the [differential here](http://web.archive.org/web/20210112045324/http://www.mkyong.com/hibernate/different-between-cascade-and-inverse/).

## 结论

理解“逆”对于优化 Hibernate 代码至关重要，它有助于避免许多不必要的更新语句，如上面的“insert and update example for inverse = false”。最后，试着记住逆=“真”的意思，这是关系所有者处理关系。

## 参考

1.  [http://simoes.org/docs/hibernate-2.1/155.html](http://web.archive.org/web/20210112045324/http://simoes.org/docs/hibernate-2.1/155.html)
2.  [http://docs . JBoss . org/hibernate/stable/core/reference/en/html/example-parent child . html](http://web.archive.org/web/20210112045324/http://docs.jboss.org/hibernate/stable/core/reference/en/html/example-parentchild.html)
3.  [http://tad tech . blogspot . com/2007/02/hibernate-when-is-inverse true-and-when . html](http://web.archive.org/web/20210112045324/https://tadtech.blogspot.com/2007/02/hibernate-when-is-inversetrue-and-when.html)

Tags : [hibernate](http://web.archive.org/web/20210112045324/https://mkyong.com/tag/hibernate/)<input type="hidden" id="mkyong-current-postId" value="3127">

### 相关文章

*   [Java . lang . classnotfoundexception:org . object web . as](/web/20210112045324/https://mkyong.com/java/java-lang-classnotfoundexception-org-objectweb-asm-type/)
*   为什么我的项目选择 Hibernate？
*   [记住序数参数是从 1 开始的！-嗨](/web/20210112045324/https://mkyong.com/hibernate/remember-that-ordinal-parameters-are-1-based-hibernatetemplate/)
*   [如何显示 hibernate sql 参数值- P6](/web/20210112045324/https://mkyong.com/hibernate/how-to-display-hibernate-sql-parameter-values-solution/)
*   [如何在 Hibernate - Logback 中配置日志记录](/web/20210112045324/https://mkyong.com/hibernate/how-to-configure-logging-in-hibernate-logback/)

*   [org . hibernate . annotation 异常:未知 Id.gene](/web/20210112045324/https://mkyong.com/hibernate/org-hibernate-annotationexception-unknown-id-generator/)
*   [如何在 Eclipse 中安装 Hibernate / JBoss 工具](/web/20210112045324/https://mkyong.com/hibernate/how-to-install-hibernate-tools-in-eclipse-ide/)
*   [如何生成 Hibernate 映射文件& annotati](/web/20210112045324/https://mkyong.com/hibernate/how-to-generate-code-with-hibernate-tools/)
*   [Maven 3+Hibernate 3.6+Oracle 11g 示例(XML](/web/20210112045324/https://mkyong.com/hibernate/maven-3-hibernate-3-6-oracle-11g-example-xml-mapping/)
*   [休眠-类型注释配置为 de](/web/20210112045324/https://mkyong.com/hibernate/hibernate-the-type-annotationconfiguration-is-deprecated/)