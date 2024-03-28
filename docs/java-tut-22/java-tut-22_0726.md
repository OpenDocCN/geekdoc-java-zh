# 使用 JoinRowSet 对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/joinrowset.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/joinrowset.html)

`JoinRowSet`实现允许您在`RowSet`对象之间创建 SQL `JOIN`，当它们未连接到数据源时。这很重要，因为它节省了必须创建一个或多个连接的开销。

下面涵盖的主题有：

+   创建 JoinRowSet 对象

+   添加 RowSet 对象

+   管理匹配列

`JoinRowSet`接口是`CachedRowSet`接口的子接口，因此继承了`CachedRowSet`对象的功能。这意味着`JoinRowSet`对象是一个断开连接的`RowSet`对象，可以在不始终连接到数据源的情况下运行。

## 创建 JoinRowSet 对象

`JoinRowSet`对象充当 SQL `JOIN`的持有者。来自`JoinSample.java`的以下示例显示了如何创建`JoinRowSet`对象：

```java
    RowSetFactory factory = RowSetProvider.newFactory();  
    try (CachedRowSet coffees = factory.createCachedRowSet();
         CachedRowSet suppliers = factory.createCachedRowSet();
         JoinRowSet jrs = factory.createJoinRowSet()) {
      coffees.setCommand("SELECT * FROM COFFEES");
      // Set connection parameters for the CachedRowSet coffees
      coffees.execute();

      suppliers.setCommand("SELECT * FROM SUPPLIERS");
      // Set connection parameters for the CachedRowSet suppliers      suppliers.execute();      

      jrs.addRowSet(coffees, "SUP_ID");
      jrs.addRowSet(suppliers, "SUP_ID");

      // ...

```

变量`jrs`在添加`RowSet`对象之前不包含任何内容。

## 添加 RowSet 对象

任何`RowSet`对象都可以添加到`JoinRowSet`对象中，只要它可以成为 SQL `JOIN`的一部分。`JdbcRowSet`对象始终连接到其数据源，可以添加，但通常通过直接与数据源操作而不是通过添加到`JoinRowSet`对象来成为`JOIN`的一部分。提供`JoinRowSet`实现的目的是使断开连接的`RowSet`对象能够成为`JOIN`关系的一部分。

The owner of The Coffee Break chain of coffee houses wants to get a list of the coffees he buys from Acme, Inc. In order to do this, the owner will have to get information from two tables, `COFFEES` and `SUPPLIERS`. In the database world before `RowSet` technology, programmers would send the following query to the database:

```java
String query =
    "SELECT COFFEES.COF_NAME " +
    "FROM COFFEES, SUPPLIERS " +
    "WHERE SUPPLIERS.SUP_NAME = Acme.Inc. " +
    "and " +
    "SUPPLIERS.SUP_ID = COFFEES.SUP_ID";

```

在`RowSet`技术的世界中，您可以在不向数据源发送查询的情况下实现相同的结果。您可以将包含两个表中数据的`RowSet`对象添加到`JoinRowSet`对象中。然后，因为所有相关数据都在`JoinRowSet`对象中，您可以对其执行查询以获取所需数据。

来自`JoinSample.testJoinRowSet`的以下代码片段创建了两个`CachedRowSet`对象，`coffees`中包含来自表`COFFEES`的数据，`suppliers`中包含来自表`SUPPLIERS`的数据。`coffees`和`suppliers`对象必须连接到数据库以执行其命令并获取数据，但完成后，它们不必重新连接以形成`JOIN`。

```java
    try (CachedRowSet coffees = factory.createCachedRowSet();
         CachedRowSet suppliers = factory.createCachedRowSet();
         JoinRowSet jrs = factory.createJoinRowSet()) {
      coffees.setCommand("SELECT * FROM COFFEES");
      coffees.setUsername(settings.userName);
      coffees.setPassword(settings.password);
      coffees.setUrl(settings.urlString);
      coffees.execute();

      suppliers.setCommand("SELECT * FROM SUPPLIERS");
      suppliers.setUsername(settings.userName);
      suppliers.setPassword(settings.password);
      suppliers.setUrl(settings.urlString);
      suppliers.execute();  
	  // ...

```

## 管理匹配列

查看`SUPPLIERS`表，您会发现 Acme, Inc. 的标识号为 101。在`COFFEES`表中，供应商标识号为 101 的咖啡有哥伦比亚咖啡和哥伦比亚无咖啡因。这两个表的信息可以进行连接，因为两个表都有一个名为`SUP_ID`的列。在 JDBC 的`RowSet`技术中，`JOIN`所基于的列`SUP_ID`被称为*匹配列*。

添加到`JoinRowSet`对象中的每个`RowSet`对象必须具有匹配列，即`JOIN`所基于的列。有两种方法可以为`RowSet`对象设置匹配列。第一种方法是将匹配列传递给`JoinRowSet`方法`addRowSet`，如下面的代码所示：

```java
jrs.addRowSet(coffees, "SUP_ID");

```

此行代码将`coffees`的`CachedRowSet`添加到`jrs`对象中，并将`coffees`的`SUP_ID`列设置为匹配列。

此时，`jrs`中只有`coffees`。下一个添加到`jrs`中的`RowSet`对象必须能够与`coffees`进行`JOIN`，这对于`suppliers`是成立的，因为两个表都有`SUP_ID`列。下面的代码将`suppliers`添加到`jrs`中，并将`SUP_ID`列设置为匹配列。

```java
jrs.addRowSet(suppliers, "SUP_ID");

```

现在`jrs`包含了`coffees`和`suppliers`之间的`JOIN`，所有者可以从中获取 Acme, Inc.供应的咖啡的名称。因为代码没有设置`JOIN`的类型，`jrs`保持内部 JOIN，这是默认值。换句话说，`jrs`中的一行组合了`coffees`中的一行和`suppliers`中的一行。它包含了`coffees`中的列以及`suppliers`中的列，对于`COFFEES.SUP_ID`列的值与`SUPPLIERS.SUP_ID`列的值匹配的行。以下代码打印出由 Acme, Inc.供应的咖啡的名称，其中`String` `supplierName`等于`Acme, Inc.`请注意，这是可能的，因为`JoinRowSet`对象`jrs`中现在包括了来自`suppliers`的`SUP_NAME`列和来自`coffees`的`COF_NAME`列。

```java
      System.out.println("Coffees bought from " + supplierName + ": ");
      while (jrs.next()) {
        if (jrs.getString("SUP_NAME").equals(supplierName)) { 
          String coffeeName = jrs.getString(1);
          System.out.println("     " + coffeeName);
        }
      }

```

这将产生类似以下的输出：

```java
Coffees bought from Acme, Inc.:
     Colombian
     Colombian_Decaf

```

`JoinRowSet`接口提供了用于设置将形成的`JOIN`类型的常量，但目前实现的唯一类型是`JoinRowSet.INNER_JOIN`。
