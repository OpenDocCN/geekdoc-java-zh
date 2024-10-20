# Hibernate 数据过滤器示例–XML 和注释

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-data-filter-example-xml-and-annotation/>

Hibernate 数据过滤器是一种创新的方式来过滤从数据库中检索的数据，以一种更可重用的方式和“可见性”规则。数据过滤器有一个唯一的名称，全局访问并接受过滤器规则的参数化值，您可以在 Hibernate 会话中启用和禁用它。

## 休眠数据过滤器示例

在本例中，它定义了一个数据过滤器，过滤指定日期的收集数据。Hibernate 数据过滤器可以在 XML 映射文件和注释中实现。

 ## 1.XML 映射文件中的 Hibernate 数据过滤器

用' **filter-def** '关键字定义一个数据过滤器，并接受一个日期参数。

```java
 <filter-def name="stockRecordFilter">
     <filter-param name="stockRecordFilterParam" type="date"/>
</filter-def> 
```

##### XML 映射示例

一个 XML 映射文件示例，用于声明并将其分配给收集组。

```java
 <hibernate-mapping>
 <class name="com.mkyong.common.Stock" table="stock" catalog="mkyong">
   ...
   <set name="stockDailyRecords" inverse="true" table="stock_daily_record">
      <key>
         <column name="STOCK_ID" not-null="true" />
      </key>
      <one-to-many class="com.mkyong.common.StockDailyRecord" />
    <filter name="stockRecordFilter" condition="date >= :stockRecordFilterParam"/>
   </set>
 </class>   

 <filter-def name="stockRecordFilter">
   <filter-param name="stockRecordFilterParam" type="date"/>
 </filter-def>
</hibernate-mapping> 
```

在**condition = " date>=:stockRecordFilterParam "**中，“日期”是属于“StockDailyRecord”的一个属性。

 ## 2.批注中的休眠数据过滤器

用' **@FilterDef** '关键字定义一个数据过滤器，用 **@ParamDef** 接受一个日期参数。

```java
 @FilterDef(name="stockRecordFilter", 
parameters=@ParamDef( name="stockRecordFilterParam", type="date" ) ) 
```

##### 注释示例

要声明并将其分配给收集组的批注文件示例。

```java
 ...
@Entity
@FilterDef(name="stockRecordFilter", 
parameters=@ParamDef( name="stockRecordFilterParam", type="date" ) )
@Table(name = "stock", catalog = "mkyong")
public class Stock implements java.io.Serializable {
         ...
	@OneToMany(fetch = FetchType.LAZY, mappedBy = "stock")
	@Filter(
		name = "stockRecordFilter",
		condition="date >= :stockRecordFilterParam"
	)
	public Set<StockDailyRecord> getStockDailyRecords() {
		return this.stockDailyRecords;
	} 
```

在**condition = " date>=:stockRecordFilterParam "**中，“日期”是属于“StockDailyRecord”的一个属性。

## 如何启用和禁用数据过滤器

启用数据过滤器。

```java
 Filter filter = session.enableFilter("stockRecordFilter");
filter.setParameter("stockRecordFilterParam", new Date()); 
```

禁用数据过滤器。

```java
 session.disableFilter("stockRecordFilter"); 
```

## 应用和实现日期过滤器

下面的代码片段展示了如何应用和实现数据过滤器。

```java
 Session session = HibernateUtil.getSessionFactory().openSession();

        System.out.println("****** Enabled Filter ******");

        Filter filter = session.enableFilter("stockRecordFilter");
        filter.setParameter("stockRecordFilterParam", new Date());

        Stock stock = (Stock)session.get(Stock.class, 2);
        Set<StockDailyRecord> sets = stock.getStockDailyRecords();

        for(StockDailyRecord sdr : sets){
		System.out.println(sdr.getDailyRecordId());
		System.out.println(sdr.getDate());
	}

        System.out.println("****** Disabled Filter ******");

        session.disableFilter("stockRecordFilter");
        //clear the loaded instance and get Stock again, for demo only
        session.evict(stock);

        Stock stock2 = (Stock)session.get(Stock.class, 2);
        Set<StockDailyRecord> sets2 = stock2.getStockDailyRecords();

        for(StockDailyRecord sdr : sets2){
		System.out.println(sdr.getDailyRecordId());
		System.out.println(sdr.getDate());
	} 
```

*输出*

```java
 ****** Enabled Filter ******
58
2010-01-31
****** Disabled Filter ******
60
2010-01-02
58
2010-01-31
63
2010-01-23
61
2010-01-03
... 
```

在本例中(包括 XML 和注释)，在启用过滤器后，它的所有“StockDailyRecord”集合都将根据参数 date 进行过滤。

*P . S filter . set parameter(" stockRecordFilterParam "，new Date())；，当前的新日期是 2010-01-27。*

[hibernate](http://web.archive.org/web/20190305140316/http://www.mkyong.com/tag/hibernate/)







