# 使用 JdbcRowSet 对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/jdbcrowset.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/jdbcrowset.html)

`JdbcRowSet` 对象是一个增强的 `ResultSet` 对象。它与数据源保持连接，就像 `ResultSet` 对象一样。最大的区别在于它具有一组属性和监听器通知机制，使其成为一个 JavaBeans 组件。

`JdbcRowSet` 对象的主要用途之一是使一个 `ResultSet` 对象在没有这些功能的情况下可滚动和可更新。

本节涵盖以下主题：

+   创建 JdbcRowSet 对象

+   默认 JdbcRowSet 对象

+   设置属性

+   使用 JdbcRowSet 对象

+   代码示例

## 创建 JdbcRowSet 对象

通过使用 `RowSetProvider` 类创建的 `RowSetFactory` 实例来创建 `JdbcRowSet` 对象。以下示例来自 `JdbcRowSetSample.java`：

```java
    RowSetFactory factory = RowSetProvider.newFactory();

    try (JdbcRowSet jdbcRs = factory.createJdbcRowSet()) {
      jdbcRs.setUrl(this.settings.urlString);
      jdbcRs.setUsername(this.settings.userName);
      jdbcRs.setPassword(this.settings.password);
      jdbcRs.setCommand("select * from COFFEES");
      jdbcRs.execute();
      // ...

```

`RowSetFactory` 接口包含创建不同类型 `RowSet` 实现的方法：

+   `createCachedRowSet`

+   `createFilteredRowSet`

+   `createJdbcRowSet`

+   `createJoinRowSet`

+   `createWebRowSet`

## 默认 JdbcRowSet 对象

使用 `RowSetFactory` 实例创建 `JdbcRowSet` 对象时，新的 `JdbcRowSet` 对象将具有以下属性：

+   `type`: `ResultSet.TYPE_SCROLL_INSENSITIVE`（具有可滚动游标）

+   `concurrency`: `ResultSet.CONCUR_UPDATABLE`（可更新）

+   `escapeProcessing`: `true`（驱动程序将执行转义处理；启用转义处理时，驱动程序将扫描任何转义语法并将其转换为特定数据库理解的代码）

+   `maxRows`: `0`（行数没有限制）

+   `maxFieldSize`: `0`（列值的字节数没有限制；仅适用于存储 `BINARY`、`VARBINARY`、`LONGVARBINARY`、`CHAR`、`VARCHAR` 和 `LONGVARCHAR` 值的列）

+   `queryTimeout`: `0`（执行查询的时间没有限制）

+   `showDeleted`: `false`（已删除的行不可见）

+   `transactionIsolation`: `Connection.TRANSACTION_READ_COMMITTED`（仅读取已提交的数据）

+   `typeMap`: `null`（与此 `RowSet` 对象一起使用的 `Connection` 对象关联的类型映射为 `null`）

你必须记住的主要事项是，`JdbcRowSet` 和所有其他 `RowSet` 对象都是可滚动和可更新的，除非你为这些属性设置了不同的值。

## 设置属性

章节 默认 JdbcRowSet 对象 列出了创建新 `JdbcRowSet` 对象时默认设置的属性。如果使用默认构造函数，必须在填充新的 `JdbcRowSet` 对象数据之前设置一些额外的属性。

要获取其数据，`JdbcRowSet`对象首先需要连接到数据库。以下四个属性保存用于获取数据库连接的信息。

+   `username`：用户作为访问权限的一部分向数据库提供的名称

+   `password`：用户的数据库密码

+   `url`：用户想要连接的数据库的 JDBC URL

+   `datasourceName`：用于检索已在 JNDI 命名服务中注册的`DataSource`对象的名称

你设置哪些属性取决于你如何进行连接。首选的方式是使用`DataSource`对象，但你可能无法将`DataSource`对象注册到 JNDI 命名服务中，这通常由系统管理员完成。因此，代码示例都使用`DriverManager`机制来获取连接，你需要使用`url`属性而不是`datasourceName`属性。

另一个你必须设置的属性是`command`属性。该属性是确定`JdbcRowSet`对象将保存什么数据的查询。例如，以下代码行使用查询设置了`command`属性，该查询生成一个包含表`COFFEES`中所有数据的`ResultSet`对象：

```java
jdbcRs.setCommand("select * from COFFEES");

```

在设置了`command`属性和连接所需的属性之后，通过调用`execute`方法，你就可以准备好通过数据填充`jdbcRs`对象。

```java
jdbcRs.execute();

```

`execute`方法在后台为你执行许多操作：

+   它使用你分配给`url`、`username`和`password`属性的值连接到数据库。

+   它执行你在`command`属性中设置的查询。

+   它将结果的数据从`ResultSet`对象读取到`jdbcRs`对象中。

## 使用 JdbcRowSet 对象

你在`JdbcRowSet`对象中更新、插入和删除行的方式与在可更新的`ResultSet`对象中更新、插入和删除行的方式相同。同样，你在`JdbcRowSet`对象中导航的方式与在可滚动的`ResultSet`对象中导航的方式相同。

Coffee Break 咖啡连锁店收购了另一家咖啡连锁店，现在拥有一个不支持结果集滚动或更新的传统数据库。换句话说，由这个传统数据库产生的任何`ResultSet`对象都没有可滚动的游标，其中的数据也无法修改。然而，通过创建一个从`ResultSet`对象中获取数据的`JdbcRowSet`对象，实际上可以使`ResultSet`对象可滚动和可更新。

如前所述，`JdbcRowSet`对象默认是可滚动和可更新的。因为其内容与`ResultSet`对象中的内容相同，对`JdbcRowSet`对象的操作等同于对`ResultSet`对象本身的操作。而且因为`JdbcRowSet`对象与数据库有持续连接，它对自己数据所做的更改也会应用到数据库中的数据。

本节涵盖以下主题：

+   导航 JdbcRowSet 对象

+   更新列值

+   插入行

+   删除行

### 导航 JdbcRowSet 对象

一个不可滚动的`ResultSet`对象只能使用`next`方法将其光标向前移动，并且只能从第一行向最后一行向前移动。然而，默认的`JdbcRowSet`对象可以使用`ResultSet`接口中定义的所有光标移动方法。

一个`JdbcRowSet`对象可以调用方法`next`，也可以调用任何其他`ResultSet`光标移动方法。例如，以下代码行将光标移动到`jdbcRs`对象的第四行，然后再移回第三行：

```java
jdbcRs.absolute(4);
jdbcRs.previous();

```

方法`previous`类似于方法`next`，可以在`while`循环中用于按顺序遍历所有行。不同之处在于你必须将光标移动到最后一行之后的位置，而`previous`将光标向前移动。

### 更新列值

你可以像更新`ResultSet`对象中的数据一样更新`JdbcRowSet`对象中的数据。

假设 Coffee Break 的老板想要提高一磅 Espresso 咖啡的价格。如果老板知道 Espresso 在`jdbcRs`对象的第三行，那么执行此操作的代码可能如下所示：

```java
jdbcRs.absolute(3);
jdbcRs.updateFloat("PRICE", 10.99f);
jdbcRs.updateRow();

```

该代码将光标移动到第三行，并将`PRICE`列的值更改为 10.99，然后使用新价格更新数据库。

调用方法`updateRow`会更新数据库，因为`jdbcRs`已经保持了与数据库的连接。对于断开连接的`RowSet`对象，情况是不同的。

### 插入行

如果 Coffee Break 连锁店的老板想要添加一种或多种咖啡到他所提供的咖啡中，那么老板需要为每种新咖啡在`COFFEES`表中添加一行，就像在`JdbcRowSetSample.java`中的以下代码片段中所做的那样。请注意，由于`jdbcRs`对象始终连接到数据库，向`JdbcRowSet`对象插入一行与向`ResultSet`对象插入一行相同：你移动光标到插入行，使用适当的更新方法为每列设置一个值，然后调用方法`insertRow`：

```java
jdbcRs.moveToInsertRow();
jdbcRs.updateString("COF_NAME", "HouseBlend");
jdbcRs.updateInt("SUP_ID", 49);
jdbcRs.updateFloat("PRICE", 7.99f);
jdbcRs.updateInt("SALES", 0);
jdbcRs.updateInt("TOTAL", 0);
jdbcRs.insertRow();

jdbcRs.moveToInsertRow();
jdbcRs.updateString("COF_NAME", "HouseDecaf");
jdbcRs.updateInt("SUP_ID", 49);
jdbcRs.updateFloat("PRICE", 8.99f);
jdbcRs.updateInt("SALES", 0);
jdbcRs.updateInt("TOTAL", 0);
jdbcRs.insertRow();

```

当你调用方法`insertRow`时，新行将被插入`jdbcRs`对象并同时插入数据库。上述代码片段经历了这个过程两次，所以两行新行被插入`jdbcRs`对象和数据库。

### 删除行

与更新数据和插入新行一样，删除行对于`JdbcRowSet`对象和`ResultSet`对象来说是一样的。老板想要停止销售最后一行在`jdbcRs`对象中的 French Roast 无咖啡因咖啡。在以下代码行中，第一行将光标移动到最后一行，第二行删除了`jdbcRs`对象和数据库中的最后一行：

```java
jdbcRs.last();
jdbcRs.deleteRow();

```

## 代码示例

示例`JdbcRowSetSample.java`执行以下操作：

+   创建一个使用执行检索`COFFEES`表中所有行的查询产生的`ResultSet`对象初始化的新`JdbcRowSet`对象

+   将光标移动到`COFFEES`表的第三行，并更新该行的`PRICE`列

+   插入两行新行，一个是`HouseBlend`，另一个是`HouseDecaf`

+   将光标移动到最后一行并将其删除
