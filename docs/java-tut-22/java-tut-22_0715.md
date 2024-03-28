# 使用 JDBC 处理 SQL 语句

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/processingsqlstatements.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/processingsqlstatements.html)

一般来说，要使用 JDBC 处理任何 SQL 语句，您需要按照以下步骤进行：

1.  建立连接。

1.  创建语句。

1.  执行查询。

1.  处理`ResultSet`对象。

1.  关闭连接。

本页使用教程示例中的方法`CoffeesTable.viewTable`来演示这些步骤。此方法输出表`COFFEES`的内容。此方法将在本教程的后续部分中更详细地讨论：

```java
  public static void viewTable(Connection con) throws SQLException {
    String query = "select COF_NAME, SUP_ID, PRICE, SALES, TOTAL from COFFEES";
    try (Statement stmt = con.createStatement()) {
      ResultSet rs = stmt.executeQuery(query);
      while (rs.next()) {
        String coffeeName = rs.getString("COF_NAME");
        int supplierID = rs.getInt("SUP_ID");
        float price = rs.getFloat("PRICE");
        int sales = rs.getInt("SALES");
        int total = rs.getInt("TOTAL");
        System.out.println(coffeeName + ", " + supplierID + ", " + price +
                           ", " + sales + ", " + total);
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

## 建立连接

首先，与要使用的数据源建立连接。数据源可以是 DBMS、传统文件系统或具有相应 JDBC 驱动程序的其他数据源。此连接由`Connection`对象表示。有关更多信息，请参见建立连接。

## 创建语句

`Statement`是表示 SQL 语句的接口。您执行`Statement`对象，它们生成`ResultSet`对象，这是表示数据库结果集的数据表。您需要一个`Connection`对象来创建`Statement`对象。

例如，`CoffeesTable.viewTable`使用以下代码创建了一个`Statement`对象：

```java
stmt = con.createStatement();

```

有三种不同类型的语句：

+   `Statement`：用于执行没有参数的简单 SQL 语句。

+   `PreparedStatement`：（扩展`Statement`。）用于预编译可能包含输入参数的 SQL 语句。有关更多信息，请参见使用预编译语句。

+   `CallableStatement:`（扩展`PreparedStatement`。）用于执行可能包含输入和输出参数的存储过程。有关更多信息，请参见存储过程。

## 执行查询

要执行查询，请调用`Statement`中的`execute`方法，如下所示：

+   `execute`：如果查询返回的第一个对象是`ResultSet`对象，则返回`true`。如果查询可能返回一个或多个`ResultSet`对象，请使用此方法。通过反复调用`Statement.getResultSet`检索查询返回的`ResultSet`对象。

+   `executeQuery`：返回一个`ResultSet`对象。

+   `executeUpdate`：返回一个整数，表示受 SQL 语句影响的行数。如果您使用`INSERT`、`DELETE`或`UPDATE` SQL 语句，请使用此方法。

例如，`CoffeesTable.viewTable`使用以下代码执行了一个`Statement`对象：

```java
ResultSet rs = stmt.executeQuery(query);

```

有关更多信息，请参见从结果集中检索和修改值。

## 处理 ResultSet 对象

通过游标访问`ResultSet`对象中的数据。请注意，此游标不是数据库游标。这个游标是指向`ResultSet`对象中一行数据的指针。最初，游标位于第一行之前。您调用`ResultSet`对象中定义的各种方法来移动游标。

例如，`CoffeesTable.viewTable`重复调用`ResultSet.next`方法来将游标向前移动一行。每次调用`next`时，该方法会输出游标当前位置的行中的数据：

```java
      ResultSet rs = stmt.executeQuery(query);
      while (rs.next()) {
        String coffeeName = rs.getString("COF_NAME");
        int supplierID = rs.getInt("SUP_ID");
        float price = rs.getFloat("PRICE");
        int sales = rs.getInt("SALES");
        int total = rs.getInt("TOTAL");
        System.out.println(coffeeName + ", " + supplierID + ", " + price +
                           ", " + sales + ", " + total);
      }
      // ...

```

查看从结果集中检索和修改值获取更多信息。

## 关闭连接

当您使用完`Connection`、`Statement`或`ResultSet`对象后，请调用其`close`方法立即释放它正在使用的资源。

或者，可以使用`try`-with-resources 语句自动关闭`Connection`、`Statement`和`ResultSet`对象，无论是否抛出`SQLException`。（当 JDBC 在与数据源交互时遇到错误时，会抛出`SQLException`。查看处理 SQL 异常获取更多信息。）自动资源语句由一个`try`语句和一个或多个声明的资源组成。例如，`CoffeesTable.viewTable`方法会自动关闭其`Statement`对象，如下所示：

```java
  public static void viewTable(Connection con) throws SQLException {
    String query = "select COF_NAME, SUP_ID, PRICE, SALES, TOTAL from COFFEES";
    try (Statement stmt = con.createStatement()) {
      ResultSet rs = stmt.executeQuery(query);
      while (rs.next()) {
        String coffeeName = rs.getString("COF_NAME");
        int supplierID = rs.getInt("SUP_ID");
        float price = rs.getFloat("PRICE");
        int sales = rs.getInt("SALES");
        int total = rs.getInt("TOTAL");
        System.out.println(coffeeName + ", " + supplierID + ", " + price +
                           ", " + sales + ", " + total);
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

以下语句是一个`try`-with-resources 语句，声明了一个资源`stmt`，当`try`块终止时将自动关闭该资源：

```java
    try (Statement stmt = con.createStatement()) {
      // ...
    }

```

查看 try-with-resources 语句在 Essential Classes 教程中获取更多信息。
