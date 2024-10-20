# 级联和逆之间的不同

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/different-between-cascade-and-inverse/>

许多 Hibernate 开发人员对 cascade 选项和 inverse 关键字感到困惑。在某些方面..它们一开始看起来真的很相似，两者都与关系有关。

## 级联与反向

然而，级联和逆之间没有关系，两者是完全不同的概念。

 ## 1.相反的

这用于决定哪一方是管理关系的关系所有者(插入或更新外键列)。

##### 例子

在此示例中，关系所有者属于 stockDailyRecords (inverse=true)。

```java
 <!-- Stock.hbm.xml -->
<hibernate-mapping>
    <class name="com.mkyong.common.Stock" table="stock" ...>
    ...
    <set name="stockDailyRecords" table="stock_daily_record" inverse="true">
        <key>
            <column name="STOCK_ID" not-null="true" />
        </key>
        <one-to-many class="com.mkyong.common.StockDailyRecord" />
    </set>
    ... 
```

当您保存或更新股票对象时

```java
 session.save(stock);
session.update(stock); 
```

Hibernate 只会插入或更新股票表，不会更新外键列。[此处有更多详细示例……](http://web.archive.org/web/20190306163129/http://www.mkyong.com/hibernate/inverse-true-example-and-explanation/)

 ## 2.串联

在 cascade 中，在一个操作(保存、更新和删除)完成后，它决定是否需要在另一个有关系实体上调用其他操作(保存、更新和删除)。

##### 例子

在本例中，在 stockDailyRecords 上声明了 cascade="save-update"。

```java
 <!-- Stock.hbm.xml -->
<hibernate-mapping>
    <class name="com.mkyong.common.Stock" table="stock" ...>
    ...
    <set name="stockDailyRecords" table="stock_daily_record" 
        cascade="save-update" inverse="true">
        <key>
            <column name="STOCK_ID" not-null="true" />
        </key>
        <one-to-many class="com.mkyong.common.StockDailyRecord" />
    </set>
    ... 
```

当您保存或更新股票对象时

```java
 session.save(stock);
session.update(stock); 
```

它会将记录插入或更新到股票表中，并在 StockDailyRecord 上调用另一个 insert 或 update 语句(cascade="save-update ")。[此处有更多详细示例……](http://web.archive.org/web/20190306163129/http://www.mkyong.com/hibernate/hibernate-cascade-example-save-update-delete-and-delete-orphan/)

## 结论

简而言之，“逆向”是决定哪一方将更新外键，而“级联”是决定应该执行什么样的后续操作。两者在关系上看起来很相似，但这完全是两码事。Hibernate 开发人员值得花时间去研究它，因为误解这个概念或误用它会给应用程序带来严重的性能或数据完整性问题。

## 参考

1.[逆= "真"例及解释](http://web.archive.org/web/20190306163129/http://www.mkyong.com/hibernate/inverse-true-example-and-explanation/)
2。[级联示例–保存、更新和删除](http://web.archive.org/web/20190306163129/http://www.mkyong.com/hibernate/hibernate-cascade-example-save-update-delete-and-delete-orphan/)

[cascade](http://web.archive.org/web/20190306163129/http://www.mkyong.com/tag/cascade/) [hibernate](http://web.archive.org/web/20190306163129/http://www.mkyong.com/tag/hibernate/)







