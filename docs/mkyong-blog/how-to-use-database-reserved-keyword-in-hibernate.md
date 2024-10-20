# Hibernate 中如何使用数据库保留关键字？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/how-to-use-database-reserved-keyword-in-hibernate/>

在 Hibernate 中，当您试图将一个对象保存到一个以任何数据库保留关键字作为列名的表中时，您可能会遇到以下错误…

```java
 ERROR JDBCExceptionReporter:78 - You have an error in your SQL syntax; 
check the manual that corresponds to your MySQL server version for the 
right syntax to use near 'Datadabase reserved keyword.... 
```

## 保留关键字“DESC”

在 MySQL 中，“DESC”是保留关键字。让我们看一些例子来演示如何在 Hibernate 中使用这个保留关键字。

 ## Hibernate XML 映射文件

这是表列的默认 XML 映射文件实现，它将导致 JDBCException…

```java
 <property name="desc" type="string" >
            <column name="DESC" length="255" not-null="true" />
        </property> 
```

 ## 解决办法

1.用方括号[]将关键字括起来。

```java
 <property name="desc" type="string" >
            <column name="[DESC]" length="255" not-null="true" />
        </property> 
```

2.使用单引号(')将双引号(")括起来

```java
 <property name="desc" type="string" >
            <column name='"DESC"' length="255" not-null="true" />
        </property> 
```

## Hibernate 注释

这是表列的默认注释实现，它将导致 JDBCException…

```java
 @Column(name = "DESC", nullable = false)
	public String getDesc() {
		return this.desc;
	} 
```

## 解决办法

1.用方括号[]将关键字括起来。

```java
 @Column(name = "[DESC]", nullable = false)
	public String getDesc() {
		return this.desc;
	} 
```

2.用双引号(")将它括起来。

```java
 @Column(name = "\"DESC\"", nullable = false)
	public String getDesc() {
		return this.desc;
	} 
```

## 结论

同样的解决方案也可以应用于保留关键字作为表名。

[hibernate](http://web.archive.org/web/20190220131228/http://www.mkyong.com/tag/hibernate/) [keyword](http://web.archive.org/web/20190220131228/http://www.mkyong.com/tag/keyword/)







