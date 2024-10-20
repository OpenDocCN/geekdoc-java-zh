# hibernate–动态插入属性示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-dynamic-insert-attribute-example/>

## 什么是动态插入

dynamic-insert 属性告诉 Hibernate 是否在 SQL INSERT 语句中包含空属性。让我们探讨一些例子来更清楚地了解它。

 ## 动态插入示例

 ## 1.动态插入=假

dynamic-insert 的默认值是 false，这意味着**在 Hibernate 的 SQL INSERT 语句中包含空属性**。

例如，尝试为一个对象属性设置一些空值并保存它。

```java
 StockTransaction stockTran = new StockTransaction();
        //stockTran.setPriceOpen(new Float("1.2"));
        //stockTran.setPriceClose(new Float("1.1"));
        //stockTran.setPriceChange(new Float("10.0"));
        stockTran.setVolume(2000000L);
        stockTran.setDate(new Date());
        stockTran.setStock(stock);

        session.save(stockTran); 
```

打开 Hibernate "show_sql "为 true，你会看到下面的 insert SQL 语句。

```java
 Hibernate: 
    insert 
    into
        mkyong.stock_transaction
        (DATE, PRICE_CHANGE, PRICE_CLOSE, PRICE_OPEN, STOCK_ID, VOLUME) 
    values
        (?, ?, ?, ?, ?, ?) 
```

**Hibernate 将为插入生成不必要的列** (PRICE_CHANGE，PRICE_CLOSE，PRICE_OPEN)。

## 2.动态插入=真

如果将 dynamic-insert 设置为 true，这意味着**在 Hibernate 的 SQL INSERT 语句中排除空属性值**。

例如，尝试将一些空值设置到一个对象属性并再次保存它。

```java
 StockTransaction stockTran = new StockTransaction();
        //stockTran.setPriceOpen(new Float("1.2"));
        //stockTran.setPriceClose(new Float("1.1"));
        //stockTran.setPriceChange(new Float("10.0"));
        stockTran.setVolume(2000000L);
        stockTran.setDate(new Date());
        stockTran.setStock(stock);

        session.save(stockTran); 
```

将 Hibernate“show _ SQL”设置为 true。您将看到不同的 insert SQL 语句。

```java
 Hibernate: 
    insert 
    into
        mkyong.stock_transaction
        (DATE, STOCK_ID, VOLUME) 
    values
        (?, ?, ?) 
```

**Hibernate 将只为插入生成必要的列**(日期、股票 ID、数量)。

## 性能问题

在某些情况下，比如一个有数百列的非常大的表(遗留设计)，或者一个表包含非常大的数据量，插入一些不必要的东西肯定会降低系统性能。

## 如何配置

您可以通过注释或 XML 映射文件配置动态插入属性值。

## 1.注释

```java
 @Entity
@Table(name = "stock_transaction", catalog = "mkyong")
@org.hibernate.annotations.Entity(
		dynamicInsert = true
)
public class StockTransaction implements java.io.Serializable { 
```

## 2.XML 映射

```java
 <class ... table="stock_transaction" catalog="mkyong" dynamic-insert="true">
        <id name="tranId" type="java.lang.Integer">
            <column name="TRAN_ID" />
            <generator class="identity" />
        </id> 
```

## 结论

这个小小的"**动态插入**"调整可能会提高您的系统性能，强烈建议这样做。但是，我脑子里的一个问题是，Hibernate 为什么默认设置为 false？

## 追踪

1.hibernate-[动态更新属性](http://web.archive.org/web/20190309054150/http://www.mkyong.com/hibernate/hibernate-dynamic-update-attribute-example/)示例

[hibernate](http://web.archive.org/web/20190309054150/http://www.mkyong.com/tag/hibernate/)







