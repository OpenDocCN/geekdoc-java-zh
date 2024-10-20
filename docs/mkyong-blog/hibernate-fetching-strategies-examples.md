# hibernate–获取策略示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-fetching-strategies-examples/>

Hibernate 几乎没有抓取策略来优化 Hibernate 生成的 select 语句，因此它可以尽可能地高效。获取策略在映射关系中声明，以定义 Hibernate 如何获取其相关的集合和实体。

## 抓取策略

有四种抓取策略

1.fetch-"join" =禁用延迟加载，总是加载所有的集合和实体。
2。fetch-“select”(默认)=惰性加载所有集合和实体。
3。batch-size="N" =提取多达“N”个集合或实体，*不记录*。
4。fetch-"subselect" =将其集合分组到一个 subselect 语句中。

关于详细的解释，你可以查看 [Hibernate 文档](http://web.archive.org/web/20190305234631/https://www.hibernate.org/315.html)。

 ## 获取策略示例

这里有一个“一对多关系”的例子来演示获取策略。一只股票是属于许多股票的日常记录。

*在 XML 文件*中声明提取策略的示例

```java
 ...
<hibernate-mapping>
    <class name="com.mkyong.common.Stock" table="stock">
        <set name="stockDailyRecords"  cascade="all" inverse="true" 
            table="stock_daily_record" batch-size="10" fetch="select">
            <key>
                <column name="STOCK_ID" not-null="true" />
            </key>
            <one-to-many class="com.mkyong.common.StockDailyRecord" />
        </set>
    </class>
</hibernate-mapping> 
```

*在注释*中声明提取策略的示例

```java
 ...
@Entity
@Table(name = "stock", catalog = "mkyong")
public class Stock implements Serializable{
...
	@OneToMany(fetch = FetchType.LAZY, mappedBy = "stock")
	@Cascade(CascadeType.ALL)
	@Fetch(FetchMode.SELECT)
        @BatchSize(size = 10)
	public Set<StockDailyRecord> getStockDailyRecords() {
		return this.stockDailyRecords;
	}
...
} 
```

让我们探讨一下获取策略如何影响 Hibernate 生成的 SQL 语句。

 ## 1.fetch="select "或@Fetch(FetchMode。选择)

这是默认的获取策略。它支持所有相关集合的延迟加载。让我们看看这个例子…

```java
 //call select from stock
Stock stock = (Stock)session.get(Stock.class, 114); 
Set sets = stock.getStockDailyRecords();

//call select from stock_daily_record
for ( Iterator iter = sets.iterator();iter.hasNext(); ) { 
      StockDailyRecord sdr = (StockDailyRecord) iter.next();
      System.out.println(sdr.getDailyRecordId());
      System.out.println(sdr.getDate());
} 
```

*输出*

```java
 Hibernate: 
    select ...from mkyong.stock
    where stock0_.STOCK_ID=?

Hibernate: 
    select ...from mkyong.stock_daily_record
    where stockdaily0_.STOCK_ID=? 
```

Hibernate 生成了两条 select 语句

1.Select 语句检索股票记录-**session . get(Stock . class，114)**
2。选择其相关集合-**sets . iterator()**

## 2.fetch="join "或@Fetch(FetchMode。加入)

“连接”抓取策略将禁用所有相关集合的延迟加载。让我们看看这个例子…

```java
 //call select from stock and stock_daily_record
Stock stock = (Stock)session.get(Stock.class, 114); 
Set sets = stock.getStockDailyRecords();

//no extra select
for ( Iterator iter = sets.iterator();iter.hasNext(); ) { 
      StockDailyRecord sdr = (StockDailyRecord) iter.next();
      System.out.println(sdr.getDailyRecordId());
      System.out.println(sdr.getDate());
} 
```

*输出*

```java
 Hibernate: 
    select ...
    from
        mkyong.stock stock0_ 
    left outer join
        mkyong.stock_daily_record stockdaily1_ 
            on stock0_.STOCK_ID=stockdaily1_.STOCK_ID 
    where
        stock0_.STOCK_ID=? 
```

Hibernate 只生成一个 select 语句，它在股票初始化时检索所有相关的集合。–**session . get(stock . class，114)**

1.Select 语句来检索股票记录并外部联接其相关集合。

## 3.batch-size="10 "或@BatchSize(size = 10)

这种“批量”获取策略总是被许多 Hibernate 开发人员误解。让我们看看这里的*误解*概念…

```java
 Stock stock = (Stock)session.get(Stock.class, 114); 
Set sets = stock.getStockDailyRecords();

for ( Iterator iter = sets.iterator();iter.hasNext(); ) { 
      StockDailyRecord sdr = (StockDailyRecord) iter.next();
      System.out.println(sdr.getDailyRecordId());
      System.out.println(sdr.getDate());
} 
```

您的预期结果是什么，是从集合中每次提取 10 条记录吗？参见
输出*输出*

```java
 Hibernate: 
    select ...from mkyong.stock
    where stock0_.STOCK_ID=?

Hibernate: 
    select ...from mkyong.stock_daily_record
    where stockdaily0_.STOCK_ID=? 
```

批量大小在这里没有任何作用，它不是批量大小如何工作的。见此说法。

> 批量获取策略没有定义集合中有多少记录被加载。相反，它定义了应该加载多少个集合。
> 
> —重复 N 次，直到你记住这句话—

##### 另一个例子

再看一个例子，你想把所有的股票记录及其相关的股票日报表(集合)一个一个打印出来。

```java
 List<Stock> list = session.createQuery("from Stock").list();

for(Stock stock : list){

    Set sets = stock.getStockDailyRecords();

    for ( Iterator iter = sets.iterator();iter.hasNext(); ) { 
            StockDailyRecord sdr = (StockDailyRecord) iter.next();
            System.out.println(sdr.getDailyRecordId());
            System.out.println(sdr.getDate());
    }
} 
```

##### 没有批量提取策略

*输出*

```java
 Hibernate: 
    select ...
    from mkyong.stock stock0_

Hibernate: 
    select ...
    from mkyong.stock_daily_record stockdaily0_ 
    where stockdaily0_.STOCK_ID=?

Hibernate: 
    select ...
    from mkyong.stock_daily_record stockdaily0_ 
    where stockdaily0_.STOCK_ID=?

Keep repeat the select statements....depend how many stock records in your table. 
```

如果数据库中有 20 条股票记录，Hibernate 的默认获取策略将生成 20+1 条 select 语句并命中数据库。

1.Select 语句检索所有股票记录。
2。选择其相关集合
3。选择其相关收藏
4。选择其相关集合
…。
21。选择其相关集合

生成的查询效率不高，导致了严重的性能问题。

##### 启用了 batch-size='10 '提取策略

让我们看另一个例子，batch-size='10 '是启用的。
*输出*

```java
 Hibernate: 
    select ...
    from mkyong.stock stock0_

Hibernate: 
    select ...
    from mkyong.stock_daily_record stockdaily0_ 
    where
        stockdaily0_.STOCK_ID in (
            ?, ?, ?, ?, ?, ?, ?, ?, ?, ?
        ) 
```

现在，Hibernate 将使用 select *in*语句预先获取集合。如果您有 20 条股票记录，它将生成 3 条 select 语句。

1.Select 语句检索所有股票记录。
2。Select In 语句预取其相关集合(一次 10 个集合)
3。Select In 语句预取其相关集合(一次 10 个集合)

启用批量大小后，它将 select 语句从 21 条 select 语句简化为 3 条 select 语句。

## 4.fetch="subselect "或@Fetch(FetchMode。子选择)

这种获取策略是在一个 sub select 语句中启用其所有相关集合。让我们再次看到相同的查询..

```java
 List<Stock> list = session.createQuery("from Stock").list();

for(Stock stock : list){

    Set sets = stock.getStockDailyRecords();

    for ( Iterator iter = sets.iterator();iter.hasNext(); ) { 
            StockDailyRecord sdr = (StockDailyRecord) iter.next();
            System.out.println(sdr.getDailyRecordId());
            System.out.println(sdr.getDate());
    }
} 
```

*输出*

```java
 Hibernate: 
    select ...
    from mkyong.stock stock0_

Hibernate: 
    select ...
    from
        mkyong.stock_daily_record stockdaily0_ 
    where
        stockdaily0_.STOCK_ID in (
            select
                stock0_.STOCK_ID 
            from
                mkyong.stock stock0_
        ) 
```

启用“子选择”后，它将创建两个 select 语句。

1.Select 语句检索所有股票记录。
2。在子选择查询中选择其所有相关集合。

## 结论

抓取策略非常灵活，是优化 Hibernate 查询的一个非常重要的调整，但是如果用错了地方，那将是一场灾难。

## 参考

1.[http://docs . JBoss . org/hibernate/core/3.3/reference/en/html/performance . html](http://web.archive.org/web/20190305234631/http://docs.jboss.org/hibernate/core/3.3/reference/en/html/performance.html)
2 .[https://www.hibernate.org/315.html](http://web.archive.org/web/20190305234631/https://www.hibernate.org/315.html)

[hibernate](http://web.archive.org/web/20190305234631/http://www.mkyong.com/tag/hibernate/)







