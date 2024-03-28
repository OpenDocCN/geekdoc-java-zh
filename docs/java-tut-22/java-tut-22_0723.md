# 使用 RowSet 对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/rowset.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/rowset.html)

JDBC `RowSet`对象以一种使其比结果集更灵活且更易于使用的方式保存表格数据。

Oracle 为`RowSet`的一些更受欢迎的用途定义了五个接口，并为这些`RowSet`接口提供了标准参考。在本教程中，您将学习如何使用这些参考实现。

这些版本的`RowSet`接口及其实现是为程序员提供方便的。程序员可以自由编写自己的`javax.sql.RowSet`接口版本，扩展这五个`RowSet`接口的实现，或编写自己的实现。然而，许多程序员可能会发现标准参考实现已经符合他们的需求，并将直接使用它们。

本节介绍了`RowSet`接口以及扩展此接口的以下接口：

+   `JdbcRowSet`

+   `CachedRowSet`

+   `WebRowSet`

+   `JoinRowSet`

+   `FilteredRowSet`

下面的主题包括：

+   RowSet 对象能做什么？

+   RowSet 对象的种类

## RowSet 对象能做什么？

所有`RowSet`对象都源自`ResultSet`接口，因此共享其功能。JDBC `RowSet`对象的特殊之处在于它们添加了以下新功能：

+   作为 JavaBeans 组件

+   添加滚动或可更新性

### 作为 JavaBeans 组件

所有`RowSet`对象都是 JavaBeans 组件。这意味着它们具有以下特性：

+   属性

+   JavaBeans 通知机制

#### 属性

所有`RowSet`对象都有属性。属性是具有对应的 getter 和 setter 方法的字段。属性暴露给构建工具（例如随 IDE 一起提供的 JDveloper 和 Eclipse），使您可以直观地操作 bean。有关更多信息，请参阅 JavaBeans 教程中的属性课程。

#### JavaBeans 通知机制

`RowSet`对象使用 JavaBeans 事件模型，其中注册的组件在发生某些事件时会收到通知。对于所有`RowSet`对象，三个事件会触发通知：

+   光标移动

+   行的更新、插入或删除

+   整个`RowSet`内容的更改

事件的通知发送给所有*监听器*，即已实现`RowSetListener`接口并已将自己添加到`RowSet`对象的组件列表中以在发生任何三个事件时收到通知的组件。

一个监听器可以是一个 GUI 组件，比如一个条形图。如果条形图正在跟踪一个`RowSet`对象中的数据，那么每当数据发生变化时，监听器都希望知道新的数据值。因此，监听器将实现`RowSetListener`方法来定义在特定事件发生时将执行什么操作。然后，监听器还必须添加到`RowSet`对象的监听器列表中。下面的代码行将条形图组件`bg`注册到`RowSet`对象`rs`。

```java
rs.addListener(bg);

```

现在，每当光标移动、行发生变化或`rs`中的所有数据都更新时，`bg`都会收到通知。

### 添加滚动性或可更新性

一些数据库管理系统不支持可以滚动（可滚动）的结果集，有些不支持可以更新（可更新）的结果集。如果该数据库管理系统的驱动程序没有添加滚动或更新结果集的功能，您可以使用`RowSet`对象来实现。`RowSet`对象默认是可滚动和可更新的，因此通过将结果集的内容填充到`RowSet`对象中，您可以有效地使结果集可滚动和可更新。

## RowSet 对象的种类

`RowSet`对象被认为是连接的或断开连接的。*连接的*`RowSet`对象使用 JDBC 驱动程序与关系数据库建立连接，并在其生命周期内保持该连接。*断开连接的*`RowSet`对象仅在从`ResultSet`对象中读取数据或将数据写回数据源时才与数据源建立连接。在从数据源读取数据或向数据源写入数据后，`RowSet`对象会断开与数据源的连接，从而变为“断开连接”。在其大部分生命周期中，断开连接的`RowSet`对象不与数据源建立连接，并且独立运行。接下来的两节将告诉您在连接或断开连接方面，`RowSet`对象可以做什么。

### 连接的 RowSet 对象

标准`RowSet`实现中只有一个连接的`RowSet`对象：`JdbcRowSet`。作为与数据库始终连接的对象，`JdbcRowSet`对象最类似于`ResultSet`对象，并且通常被用作包装器，使本来不可滚动和只读的`ResultSet`对象变得可滚动和可更新。

作为 JavaBeans 组件，例如，`JdbcRowSet`对象可以在 GUI 工具中用于选择 JDBC 驱动程序。`JdbcRowSet`对象可以这样使用，因为它实际上是驱动程序的包装器，该驱动程序获取了与数据库的连接。

### 断开连接的 RowSet 对象

另外四个实现是断开连接的`RowSet`实现。断开连接的`RowSet`对象具有连接的`RowSet`对象的所有功能，还具有仅适用于断开连接的`RowSet`对象的附加功能。例如，不必维护与数据源的连接使得断开连接的`RowSet`对象比`JdbcRowSet`对象或`ResultSet`对象更轻量级。断开连接的`RowSet`对象也是可序列化的，而且既可序列化又轻量级的组合使它们非常适合通过网络发送数据。它们甚至可以用于向 PDA 和手机等轻客户端发送数据。

`CachedRowSet`接口定义了所有断开连接的`RowSet`对象可用的基本功能。其他三个是`CachedRowSet`接口的扩展，提供更专业的功能。以下信息显示了它们之间的关系：

一个`CachedRowSet`对象具有`JdbcRowSet`对象的所有功能，还可以执行以下操作：

+   获取到数据源的连接并执行查询

+   从生成的`ResultSet`对象中读取数据并用该数据填充自身

+   在断开连接时操作数据并对数据进行更改

+   重新连接到数据源以将更改写回

+   检查与数据源的冲突并解决这些冲突

一个`WebRowSet`对象具有`CachedRowSet`对象的所有功能，还可以执行以下操作：

+   将自身写为 XML 文档

+   读取描述`WebRowSet`对象的 XML 文档

一个`JoinRowSet`对象具有`WebRowSet`对象（因此也具有`CachedRowSet`对象）的所有功能，还可以执行以下操作：

+   形成等效于`SQL JOIN`的操作而无需连接到数据源

一个`FilteredRowSet`对象同样具有`WebRowSet`对象（因此也具有`CachedRowSet`对象）的所有功能，还可以执行以下操作：

+   应用过滤条件，以便只有选定的数据可见。这相当于在`RowSet`对象上执行查询，而无需使用查询语言或连接到数据源。
