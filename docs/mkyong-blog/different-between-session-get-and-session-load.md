# session.get()和 session.load()不同

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/different-between-session-get-and-session-load/>

很多时候，你会注意到 Hibernate 开发人员混合使用 **session.get()** 和 **session load()** ，你想知道有什么不同吗？什么时候你应该使用其中的一个？

## session.get()和 session.load()不同

当然，实际上，这两个函数都是用不同的机制来检索一个对象。

## 1.session.load()

*   它将总是返回一个“**代理**”(Hibernate 术语)，而不会命中数据库。在 Hibernate 中，代理是一个具有给定标识符值的对象，它的属性还没有初始化，看起来只是一个临时的假对象。
*   如果没有找到行，它将抛出一个 **ObjectNotFoundException** 。

## 2.session.get()

*   它总是**命中数据库**并返回真实对象，一个代表数据库行的对象，而不是代理。
*   如果没有找到行，它返回空值。

## 这是性能的问题

Hibernate 创建任何东西都是有原因的，当你做关联的时候，正常的做法是从数据库获取检索一个对象(持久实例)并把它作为一个引用分配给另一个对象，只是为了维护关系。下面我们通过一些例子来了解在什么情况下应该使用 **session.load()** 。

## 1.session.get()

例如，在一个股票应用程序中，股票和 StockTransactions 应该有一个“一对多”的关系，当您想要保存一个股票交易时，通常会声明如下所示的内容

```java
 Stock stock = (Stock)session.get(Stock.class, new Integer(2));
           StockTransaction stockTransactions = new StockTransaction();
           //set stockTransactions detail
           stockTransactions.setStock(stock);        
           session.save(stockTransactions); 
```

## 输出

```java
 Hibernate: 
    select ... from mkyong.stock stock0_ 
    where stock0_.STOCK_ID=?
Hibernate: 
    insert into mkyong.stock_transaction (...) 
    values (?, ?, ?, ?, ?, ?) 
```

在 session.get()中，Hibernate 会命中数据库检索 Stock 对象，并把它作为 StockTransaction 的引用。但是，这个保存过程要求极高，每小时可能有上千或上百万笔交易，你认为有必要打数据库检索股票对象的所有内容保存一条股票交易记录吗？毕竟，您只需要股票的 Id 作为 StockTransaction 的参考。

## 2.session.load()

在上面的场景中， **session.load()** 将是您的好解决方案，让我们来看看这个例子。

```java
 Stock stock = (Stock)session.load(Stock.class, new Integer(2));
           StockTransaction stockTransactions = new StockTransaction();
           //set stockTransactions detail
           stockTransactions.setStock(stock);        
           session.save(stockTransactions); 
```

## 输出

```java
 Hibernate: 
    insert into mkyong.stock_transaction (...) 
    values (?, ?, ?, ?, ?, ?) 
```

在 session.load()中，Hibernate 不会命中数据库(输出中没有 select 语句)来检索股票对象，它将返回一个股票代理对象——一个具有给定标识值的假对象。在这种情况下，一个代理对象足以保存股票交易记录。

## 例外

在例外情况下，请参见示例

## session.load()

```java
 Stock stock = (Stock)session.load(Stock.class, new Integer(100)); //proxy

 //initialize proxy, no row for id 100, throw ObjectNotFoundException
System.out.println(stock.getStockCode()); 
```

它将始终返回一个具有给定标识值的代理对象，即使该标识值在数据库中不存在。但是，当您试图通过从数据库中检索代理的属性来初始化代理时，它将使用 select 语句来访问数据库。如果没有找到行，将抛出一个 **ObjectNotFoundException** 。

```java
 org.hibernate.ObjectNotFoundException: No row with the given identifier exists: 
[com.mkyong.common.Stock#100] 
```

## session.get()

```java
 //return null if not found
Stock stock = (Stock)session.get(Stock.class, new Integer(100)); 
System.out.println(stock.getStockCode()); //java.lang.NullPointerException 
```

如果在数据库中找不到标识值，它将始终返回 null。

## 结论

没有永远正确的解决方案，您必须了解它们之间的差异，并决定哪种方法最适合您的应用程序。

Tags : [hibernate](http://web.archive.org/web/20210110113921/https://mkyong.com/tag/hibernate/)<input type="hidden" id="mkyong-current-postId" value="3311">

### 相关文章

*   [Java . lang . classnotfoundexception:org . object web . as](/web/20210110113921/https://mkyong.com/java/java-lang-classnotfoundexception-org-objectweb-asm-type/)
*   为什么我的项目选择 Hibernate？
*   [记住序数参数是从 1 开始的！-嗨](/web/20210110113921/https://mkyong.com/hibernate/remember-that-ordinal-parameters-are-1-based-hibernatetemplate/)
*   [如何显示 hibernate sql 参数值- P6](/web/20210110113921/https://mkyong.com/hibernate/how-to-display-hibernate-sql-parameter-values-solution/)
*   [如何在 Hibernate - Logback 中配置日志记录](/web/20210110113921/https://mkyong.com/hibernate/how-to-configure-logging-in-hibernate-logback/)

*   [org . hibernate . annotation 异常:未知 Id.gene](/web/20210110113921/https://mkyong.com/hibernate/org-hibernate-annotationexception-unknown-id-generator/)
*   [如何在 Eclipse 中安装 Hibernate / JBoss 工具](/web/20210110113921/https://mkyong.com/hibernate/how-to-install-hibernate-tools-in-eclipse-ide/)
*   [如何生成 Hibernate 映射文件& annotati](/web/20210110113921/https://mkyong.com/hibernate/how-to-generate-code-with-hibernate-tools/)
*   [Maven 3+Hibernate 3.6+Oracle 11g 示例(XML](/web/20210110113921/https://mkyong.com/hibernate/maven-3-hibernate-3-6-oracle-11g-example-xml-mapping/)
*   [休眠-类型注释配置为 de](/web/20210110113921/https://mkyong.com/hibernate/hibernate-the-type-annotationconfiguration-is-deprecated/)