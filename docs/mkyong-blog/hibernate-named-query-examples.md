# Hibernate 命名查询示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-named-query-examples/>

很多时候，开发人员喜欢将 HQL 字符串分散在 Java 代码中，这种方法很难维护，而且看起来很难看。幸运的是，Hibernate 出来了一种叫做“ **names queries** ”的技术，它让开发者把所有的 HQL 放到 XML 映射文件中或者通过注释。

## 如何声明命名查询

HQL 或本机 SQL 都支持命名查询。查看示例…

 ## 1.XML 映射文件

映射文件中的 HQL

```java
 <!-- stock.hbm.xml -->
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

    <query name="findStockByStockCode">
        <![CDATA[from Stock s where s.stockCode = :stockCode]]>
    </query>

</hibernate-mapping> 
```

映射文件中的本机 SQL

```java
 <!-- stock.hbm.xml -->
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

    <sql-query name="findStockByStockCodeNativeSQL">
	<return alias="stock" class="com.mkyong.common.Stock"/>
	<![CDATA[select * from stock s where s.stock_code = :stockCode]]>
    </sql-query>
</hibernate-mapping> 
```

您可以在' **hibernate-mapping** 元素中放置一个命名查询，但是不要放在'**类**元素之前，hibernate 会提示无效的映射文件，您的所有命名查询都必须放在'**类**元素之后。

**Note**
Regarding the CDATA , it’s always good practice to wrap your query text with CDATA, so that the XML parser will not prompt error for some special XML characters like ‘>’ , <‘ and etc. ## 2.注释

注释中的 HQL

```java
 @NamedQueries({
	@NamedQuery(
	name = "findStockByStockCode",
	query = "from Stock s where s.stockCode = :stockCode"
	)
})
@Entity
@Table(name = "stock", catalog = "mkyong")
public class Stock implements java.io.Serializable {
... 
```

批注中的本机 SQL

```java
 @NamedNativeQueries({
	@NamedNativeQuery(
	name = "findStockByStockCodeNativeSQL",
	query = "select * from stock s where s.stock_code = :stockCode",
        resultClass = Stock.class
	)
})
@Entity
@Table(name = "stock", catalog = "mkyong")
public class Stock implements java.io.Serializable {
... 
```

在原生 SQL 中，你必须声明' **resultClass** '来让 Hibernate 知道什么是返回类型，否则会导致异常“**org . Hibernate . CFG . notyeimplemented exception:尚不支持纯原生标量查询**”。

## 调用命名查询

在 Hibernate 中，可以通过 **getNamedQuery** 方法调用命名查询。

```java
 Query query = session.getNamedQuery("findStockByStockCode")
.setString("stockCode", "7277"); 
```

```java
 Query query = session.getNamedQuery("findStockByStockCodeNativeSQL")
.setString("stockCode", "7277"); 
```

## 结论

命名查询是全局访问，这意味着查询的名称在 XML 映射文件或注释中必须是唯一的。在实际环境中，将所有命名查询隔离到它们自己的文件中总是一个好的做法。此外，存储在 Hibernate 映射文件或注释中的命名查询比分散在 Java 代码中的查询更容易维护。

[hibernate](http://web.archive.org/web/20190308011751/http://www.mkyong.com/tag/hibernate/)







