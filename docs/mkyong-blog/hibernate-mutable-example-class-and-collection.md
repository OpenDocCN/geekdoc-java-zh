# Hibernate 可变示例(类和集合)

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-mutable-example-class-and-collection/>

在 hibernate 中，类及其相关集合中的“**可变的**”默认为“真”,这意味着允许添加、更新和删除类或集合。另一方面，如果可变变量被更改为 false，它在类及其相关集合中具有不同的含义。下面我们举几个例子来深入了解一下。

## Hibernate 一对多示例

我将把这个[一对多示例](http://web.archive.org/web/20221023054402/http://www.mkyong.com/hibernate/hibernate-one-to-many-relationship-example/)用于可变演示。在这个映射文件中，一个股票属于许多 StockDailyRecord。

```java
 <!-- Stock.hbm.xml -->
...
<hibernate-mapping>
    <class name="com.mkyong.common.Stock" table="stock" >
        <set name="stockDailyRecords" mutable="false" cascade="all" 
               inverse="true" lazy="true" table="stock_daily_record">
            <key>
                <column name="STOCK_ID" not-null="true" />
            </key>
            <one-to-many class="com.mkyong.common.StockDailyRecord" />
        </set>
    </class>
...    
</hibernate-mapping> 
```

## 如何声明 mutable？

XML 映射文件和注释都支持“可变”。

## 1.XML 映射文件

在映射文件中，‘**可变**关键字用于实现可变函数。

```java
 <!-- Stock.hbm.xml -->
...
<hibernate-mapping>
    <class name="com.mkyong.common.Stock" table="stock" mutable="false" >
        <set name="stockDailyRecords" mutable="false" cascade="all" 
               inverse="true" lazy="true" table="stock_daily_record" >
            <key>
                <column name="STOCK_ID" not-null="true" />
            </key>
            <one-to-many class="com.mkyong.common.StockDailyRecord" />
        </set>
    </class>
...    
</hibernate-mapping> 
```

## 2.注释

在注释中，关键字被更改为@Immutable (mutable='false ')。

```java
 ...
@Entity
@Immutable
@Table(name = "stock", catalog = "mkyong")
public class Stock implements java.io.Serializable {
...
        @OneToMany(fetch = FetchType.LAZY, mappedBy = "stock")
	@Immutable
	public Set<StockDailyRecord> getStockDailyRecords() {
		return this.stockDailyRecords;
	}
... 
```

## 在类中可变

如果在 class 元素中声明了 mutable = "false "或@Immutable，这意味着对该类的**更新将被忽略，但不会抛出异常，只允许添加和删除操作**。

## 1.测试插件

```java
 Stock stock = new Stock();
stock.setStockCode("7277");
stock.setStockName("DIALOG");
session.save(stock); 
```

如果在类中声明了 mutable = "true "(默认)或 no @Immutable。
*输出*

```java
 Hibernate: 
    insert into mkyong.stock (STOCK_CODE, STOCK_NAME) 
    values (?, ?) 
```

如果在类中声明了 mutable = "false "或@Immutable。
*输出*

```java
 Hibernate: 
    insert into mkyong.stock (STOCK_CODE, STOCK_NAME) 
    values (?, ?) 
```

**类中的可变变量对“插入”操作没有影响。**

## 2.测试更新

```java
 Stock stock = (Stock)session.createQuery(
      " from Stock where stockCode = '7277'").list().get(0);
stock.setStockName("DIALOG123");
session.saveOrUpdate(stock); 
```

如果在类中声明了 mutable = "true "或 no @Immutable。
*输出*

```java
 Hibernate: 
    select ...from mkyong.stock stock0_ 
    where stock0_.STOCK_CODE='7277'

Hibernate: 
    update mkyong.stock 
    set STOCK_CODE=?,STOCK_NAME=? 
    where STOCK_ID=? 
```

如果在类中声明了 mutable = "false "或@Immutable。
*输出*

```java
 Hibernate: 
    select ...from mkyong.stock stock0_ 
    where stock0_.STOCK_CODE='7277' 
```

**类中的可变变量不允许应用程序对其进行更新,“更新”操作将被忽略，并且不会引发异常**

## 3.测试删除

```java
 Stock stock = (Stock)session.createQuery(
      " from Stock where stockCode = '7277'").list().get(0);
session.delete(stock); 
```

如果在类中声明了 mutable = "true "(默认)或 no @Immutable。
*输出*

```java
 Hibernate: 
    delete from mkyong.stock 
    where STOCK_ID=? 
```

如果在类中声明了 mutable = "false "或@Immutable。
*输出*

```java
 Hibernate: 
    delete from mkyong.stock 
    where STOCK_ID=? 
```

**类中的可变变量在“删除”操作中无效。**

## 集合中可变的

如果在集合中声明了 mutable = "false "或@Immutable，这意味着在这个集合中不允许使用 **add 和 delete-orphan，只有 update 和' cascade delete all '是允许的**。

## 1.测试插件

假设级联插入已启用。

```java
 Stock stock = (Stock)session.createQuery(
      " from Stock where stockCode = '7277'").list().get(0);
StockDailyRecord sdr = new StockDailyRecord();
sdr.setDate(new Date());        
sdr.setStock(stock);
stock.getStockDailyRecords().add(sdr);
session.save(stock); 
```

如果在集合中声明了 mutable = "true "(默认值)或 no @Immutable。
*输出*

```java
 Hibernate: 
    insert into mkyong.stock_daily_record
    (STOCK_ID, PRICE_OPEN, PRICE_CLOSE, PRICE_CHANGE, VOLUME, DATE) 
    values (?, ?, ?, ?, ?, ?) 
```

如果在集合中声明了 mutable = "false "或@Immutable。
*输出*

```java
 Exception in thread "main" org.hibernate.HibernateException: 
changed an immutable collection instance: 
[com.mkyong.common.Stock.stockDailyRecords#111] 
```

**集合中的 Mutable 不允许' add '操作，会抛出异常。**

## 2.测试更新

假设级联更新已启用。

```java
 Stock stock = (Stock)session.createQuery(
      " from Stock where stockCode = '7277'").list().get(0);
StockDailyRecord sdr = stock.getStockDailyRecords().iterator().next();
sdr.setPriceChange(new Float(1.30));
session.saveOrUpdate(stock); 
```

如果在集合中声明了 mutable = "true "(默认值)或 no @Immutable。
*输出*

```java
 Hibernate: 
    update mkyong.stock_daily_record 
    set PRICE_CHANGE=?, ...
    where DAILY_RECORD_ID=? 
```

如果在集合中声明了 mutable = "false "或@Immutable。
*输出*

```java
 Hibernate: 
    update mkyong.stock_daily_record 
    set PRICE_CHANGE=?, ...
    where DAILY_RECORD_ID=? 
```

**集合中的可变变量对“更新”操作没有影响。**

## 3.测试删除-孤立

假设[级联删除-孤立](http://web.archive.org/web/20221023054402/http://www.mkyong.com/hibernate/hibernate-cascade-example-save-update-delete-and-delete-orphan/)被启用。

```java
 Stock stock = (Stock)session.createQuery(
      " from Stock where stockCode = '7277'").list().get(0);
StockDailyRecord sdr = stock.getStockDailyRecords().iterator().next();
stock.getStockDailyRecords().remove(sdr);
session.saveOrUpdate(stock); 
```

如果在集合中声明了 mutable = "true "(默认值)或 no @Immutable。
*输出*

```java
 Hibernate: 
    delete from mkyong.stock_daily_record 
    where DAILY_RECORD_ID=? 
```

如果在集合中声明了 mutable = "false "或@Immutable。
*输出*

```java
 Exception in thread "main" org.hibernate.HibernateException: 
changed an immutable collection instance: 
[com.mkyong.common.Stock.stockDailyRecords#111] 
```

**集合中的可变不允许“删除-孤立”操作，将引发异常。**

## 4.测试删除

假设级联删除已启用。

```java
 Stock stock = (Stock)session.createQuery(
      " from Stock where stockCode = '7277'").list().get(0);
session.saveOrUpdate(stock); 
```

如果在集合中声明了 mutable = "true "(默认值)或 no @Immutable。
*输出*

```java
 Hibernate: 
    delete from mkyong.stock_daily_record 
    where DAILY_RECORD_ID=?

Hibernate: 
    delete from mkyong.stock 
    where STOCK_ID=? 
```

如果在集合中声明了 mutable = "false "或@Immutable。
*输出*

```java
 Hibernate: 
    delete from mkyong.stock_daily_record 
    where DAILY_RECORD_ID=?

Hibernate: 
    delete from mkyong.stock 
    where STOCK_ID=? 
```

**集合中的可变元素在“删除”操作中无效，如果父元素被删除，它的所有子元素也会被删除，即使它是可变的。**

## 为什么可变？

Mutable 可以避免许多无意的数据库操作，比如添加、更新或删除一些不应该的记录。此外，根据 Hibernate 文档，mutable do 有一些小的性能优化，建议分析映射关系并根据需要实现 mutable。

## 摘要

##### 1.在类中声明了 mutable = "false "或@Immutable

这意味着对这个类的更新将被忽略，但是不会抛出异常，只允许添加和删除操作。

*在具有 mutable = " false "–insert = allow，delete=allow，update=not allow 的类中*

##### 2.集合中声明了 mutable = "false "或@Immutable

这意味着在这个集合中不允许添加和删除孤儿，只允许更新。但是，如果级联删除被启用，当父代被删除时，它的所有子代也会被删除，即使它是可变的。

*在集合中使用 mutable = " false "–insert =不允许，delete-orphan =不允许，delete =允许，update =允许*

## 完全不可改变？

一个类可以对任何动作完全不可变吗？是的，把一个 mutable =“false”放在它的所有关系中(insert =不允许，delete-orphan =不允许)，把一个 mutable =“false”放在你希望不可变的类中(update =不允许)。现在，您有了一个完全不可变的类，但是，如果级联删除选项被启用，当您的不可变类的父类被删除时，您的不可变类也将被删除。

## 参考

1.[http://docs . JBoss . org/hibernate/stable/annotations/reference/en/html _ single/](http://web.archive.org/web/20221023054402/http://docs.jboss.org/hibernate/stable/annotations/reference/en/html_single/)

<input type="hidden" id="mkyong-current-postId" value="3344">