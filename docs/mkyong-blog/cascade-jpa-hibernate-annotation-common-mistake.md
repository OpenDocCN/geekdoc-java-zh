# cascade–JPA & Hibernate 注释常见错误

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/cascade-jpa-hibernate-annotation-common-mistake/>

很多时候，开发人员混合使用 JPA 和 Hibernate 注释，这将导致一个非常常见的错误——JPA 级联类型注释在 Hibernate 中不起作用？

在代码审查阶段，我发现许多 Java 开发人员没有意识到这个错误，导致程序无法对相关实体执行级联操作。我将以这个[一对多 hibernate 示例](http://web.archive.org/web/20220930231922/http://www.mkyong.com/hibernate/hibernate-one-to-many-relationship-example/)为例进行演示。

## 错误

在一对多示例中，许多开发人员声明 JPA 级联选项如下:

```java
 ...
@Entity
@Table(name = "stock", catalog = "mkyong", uniqueConstraints = {
		@UniqueConstraint(columnNames = "STOCK_NAME"),
		@UniqueConstraint(columnNames = "STOCK_CODE") })
public class Stock implements java.io.Serializable {
    ...
    private Set<StockDailyRecord> stockDailyRecords = 
                                              new HashSet<StockDailyRecord>(0);

    @OneToMany(fetch = FetchType.LAZY, 
       cascade = {CascadeType.PERSIST,CascadeType.MERGE }, 
       mappedBy = "stock")
    public Set<StockDailyRecord> getStockDailyRecords() {
	return this.stockDailyRecords;
    }

    public void setStockDailyRecords(Set<StockDailyRecord> stockDailyRecords) {
	this.stockDailyRecords = stockDailyRecords;
    }
    ... 
```

用 Hibernate 会话保存它。

```java
 stockDailyRecords.setStock(stock);        
        stock.getStockDailyRecords().add(stockDailyRecords);

        session.save(stock);
        session.getTransaction().commit(); 
```

这段代码试图做的是，当您保存一个“股票”时，它也会保存相关的 stockDailyRecords。一切看起来都很好，但是**这不起作用**，级联选项将不会执行和保存 stockDailyRecords。你能发现这个错误吗？

## 说明

在代码中， **@OneToMany** 来自 JPA，它期望一个 JPA 级联-**javax . persistence . cascadetype**。然而，当你用 Hibernate 会话保存它时，**org . Hibernate . engine . cascade**会做如下检查…

```java
 if ( style.doCascade( action ) ) {
		cascadeProperty(
	          persister.getPropertyValue( parent, i, entityMode ),
		  types[i],
    	          style,
	          anything,
	          false
		);
	} 
```

Hibernate save 进程会导致一个 **ACTION_SAVE_UPDATE** 动作，但是 JPA 会传递一个 **ACTION_PERSIST** 和 **ACTION_MERGE** ，它们不匹配，导致级联执行失败。

@见源代码

*   **org . hibernate . engine . cascade**
*   **org . hibernate . engine . cascade style**
*   **org . hibernate . engine . cascading action**

## 解决办法

删除 JPA cascade-**javax . persistence . cascade type**，替换为 Hibernate cascade-**org . Hibernate . annotations . cascade**，替换为 **CascadeType。保存 _ 更新**。

```java
 ...
@Entity
@Table(name = "stock", catalog = "mkyong", uniqueConstraints = {
		@UniqueConstraint(columnNames = "STOCK_NAME"),
		@UniqueConstraint(columnNames = "STOCK_CODE") })
public class Stock implements java.io.Serializable {
    ...
    private Set<StockDailyRecord> stockDailyRecords = 
                                              new HashSet<StockDailyRecord>(0);

    @OneToMany(fetch = FetchType.LAZY, mappedBy = "stock")
    @Cascade({CascadeType.SAVE_UPDATE})
    public Set<StockDailyRecord> getStockDailyRecords() {
	return this.stockDailyRecords;
    }

    public void setStockDailyRecords(Set<StockDailyRecord> stockDailyRecords) {
	this.stockDailyRecords = stockDailyRecords;
    }
    ... 
```

现在，它如你所料地工作了。

## 结论

JPA 和 Hibernate cascade 注释之间似乎存在不兼容的问题，如果 Hibernate 是 JPA 的实现，是什么导致了两者之间的误解？

<input type="hidden" id="mkyong-current-postId" value="3251">