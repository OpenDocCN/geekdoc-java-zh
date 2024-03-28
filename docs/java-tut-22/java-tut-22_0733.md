# 使用 DISTINCT 数据类型

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/distinct.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/distinct.html)

**注意**：MySQL 和 Java DB 目前不支持`DISTINCT` SQL 数据类型。因此，在本节描述的功能没有 JDBC 教程示例可用来演示。

`DISTINCT` 数据类型与其他高级 SQL 数据类型的行为不同。作为一个基于已有内置类型之一的用户定义类型，它没有接口作为其在 Java 编程语言中的映射。相反，`DISTINCT` 数据类型的标准映射是其底层 SQL 数据类型映射到的 Java 类型。

为了说明，创建一个`DISTINCT`数据类型，然后看看如何检索、设置或更新它。假设你总是使用两个字母的缩写表示一个州，并且想要创建一个用于这些缩写的`DISTINCT`数据类型。你可以使用以下 SQL 语句定义你的新`DISTINCT`数据类型：

```java
CREATE TYPE STATE AS CHAR(2);

```

一些数据库使用另一种语法来创建`DISTINCT`数据类型，如下面的代码行所示：

```java
CREATE DISTINCT TYPE STATE AS CHAR(2);

```

如果一个语法不起作用，你可以尝试另一个。或者，你可以查看你的驱动程序文档，以查看它期望的确切语法。

这些语句创建了一个新的数据类型`STATE`，它可以作为列值或作为 SQL 结构化类型的属性值使用。因为`STATE`类型的值实际上是两个`CHAR`类型的值，所以你使用与检索`CHAR`值相同的方法来检索它，即`getString`。例如，假设`ResultSet *rs*`的第四列存储了`STATE`类型的值，下面的代码行检索了它的值：

```java
String state = rs.getString(4);

```

同样，你会使用`setString`方法将`STATE`值存储在数据库中，使用`updateString`方法修改其值。
