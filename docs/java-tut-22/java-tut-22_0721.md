# 使用预编译语句

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html)

本页涵盖以下主题：

+   预编译语句概述

+   创建一个 PreparedStatement 对象

+   为 PreparedStatement 参数提供值

## 预编译语句概述

有时，使用`PreparedStatement`对象向数据库发送 SQL 语句更加方便。这种特殊类型的语句源自你已经了解的更一般的`Statement`类。

如果你想多次执行一个`Statement`对象，通常使用`PreparedStatement`对象可以减少执行时间。

`PreparedStatement`对象的主要特点是，与`Statement`对象不同，它在创建时就被赋予了一个 SQL 语句。这样做的好处是，在大多数情况下，这个 SQL 语句会立即发送到数据库管理系统（DBMS）中进行编译。因此，`PreparedStatement`对象不仅包含一个 SQL 语句，而且包含一个已经预编译过的 SQL 语句。这意味着当执行`PreparedStatement`时，DBMS 可以直接运行`PreparedStatement`的 SQL 语句，而无需先进行编译。

尽管你可以使用`PreparedStatement`对象执行没有参数的 SQL 语句，但通常你最常使用它们来执行带有参数的 SQL 语句。使用带有参数的 SQL 语句的优点是，你可以多次执行相同的语句，并每次执行时提供不同的值。以下部分中有相关示例。

然而，预编译语句最重要的优点是可以帮助防止 SQL 注入攻击。SQL 注入是一种恶意利用应用程序中使用客户端提供的数据的技术，用于在 SQL 语句中执行意外命令。攻击者通过提供经过特殊设计的字符串输入来欺骗 SQL 引擎，从而未经授权地访问数据库以查看或操纵受限数据。所有 SQL 注入技术都利用应用程序中的一个漏洞：未正确验证或未验证的字符串文字被连接到动态构建的 SQL 语句中，并被 SQL 引擎解释为代码。预编译语句始终将客户端提供的数据视为参数的内容，而不是 SQL 语句的一部分。有关更多信息，请参阅 Oracle 数据库文档中的*数据库 PL/SQL 语言参考*部分中的[SQL 注入](https://docs.oracle.com/en/database/oracle/oracle-database/20/lnpls/dynamic-sql.html#GUID-1E31057E-057F-4A53-B1DD-8BC2C337AA2C)部分。

以下方法，`CoffeesTable.updateCoffeeSales`，将当前周内销售的咖啡磅数存储在每种咖啡的`SALES`列中，并更新每种咖啡的`TOTAL`列中销售的咖啡总磅数：

```java
  public void updateCoffeeSales(HashMap<String, Integer> salesForWeek) throws SQLException {
    String updateString =
      "update COFFEES set SALES = ? where COF_NAME = ?";
    String updateStatement =
      "update COFFEES set TOTAL = TOTAL + ? where COF_NAME = ?";

    try (PreparedStatement updateSales = con.prepareStatement(updateString);
         PreparedStatement updateTotal = con.prepareStatement(updateStatement))

    {
      con.setAutoCommit(false);
      for (Map.Entry<String, Integer> e : salesForWeek.entrySet()) {
        updateSales.setInt(1, e.getValue().intValue());
        updateSales.setString(2, e.getKey());
        updateSales.executeUpdate();

        updateTotal.setInt(1, e.getValue().intValue());
        updateTotal.setString(2, e.getKey());
        updateTotal.executeUpdate();
        con.commit();
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
      if (con != null) {
        try {
          System.err.print("Transaction is being rolled back");
          con.rollback();
        } catch (SQLException excep) {
          JDBCTutorialUtilities.printSQLException(excep);
        }
      }
    }
  }

```

## 创建 PreparedStatement 对象

以下创建了一个接受两个输入参数的`PreparedStatement`对象：

```java
    String updateString =
      "update COFFEES " + "set SALES = ? where COF_NAME = ?";
	// ...
    PreparedStatement updateSales = con.prepareStatement(updateString);

```

## 为 PreparedStatement 参数提供值

在执行`PreparedStatement`对象之前，必须在问号占位符的位置提供值（如果有的话）。通过调用`PreparedStatement`类中定义的 setter 方法来实现。以下语句为名为`updateSales`的`PreparedStatement`提供了两个问号占位符的值：

```java
updateSales.setInt(1, e.getValue().intValue());
updateSales.setString(2, e.getKey());

```

这些 setter 方法的每个第一个参数指定了问号占位符。在这个例子中，`setInt`指定了第一个占位符，`setString`指定了第二个占位符。

在为参数设置了值之后，它会保留该值，直到被重置为另一个值，或者调用方法`clearParameters`。使用`PreparedStatement`对象`updateSales`，以下代码片段演示了在重置其参数值并保持另一个参数值不变后重用准备好的语句：

```java
// changes SALES column of French Roast
//row to 100

updateSales.setInt(1, 100);
updateSales.setString(2, "French_Roast");
updateSales.executeUpdate();

// changes SALES column of Espresso row to 100
// (the first parameter stayed 100, and the second
// parameter was reset to "Espresso")

updateSales.setString(2, "Espresso");
updateSales.executeUpdate();

```

### 使用循环设置值

通常可以通过使用`for`循环或`while`循环为输入参数设置值，从而使编码更加简单。

`CoffeesTable.updateCoffeeSales`方法使用 for-each 循环重复设置`updateSales`和`updateTotal`中的值：

```java
for (Map.Entry<String, Integer> e : salesForWeek.entrySet()) {
  updateSales.setInt(1, e.getValue().intValue());
  updateSales.setString(2, e.getKey());
  // ...
}

```

方法`CoffeesTable.updateCoffeeSales`接受一个参数，`HashMap`。`HashMap`参数中的每个元素都包含当前周内销售的一种咖啡的名称和该种咖啡的磅数。for-each 循环遍历`HashMap`参数的每个元素，并设置`updateSales`和`updateTotal`中相应的问号占位符。

## 执行 PreparedStatement 对象

与`Statement`对象一样，要执行`PreparedStatement`对象，调用一个执行语句：如果查询只返回一个`ResultSet`（如`SELECT` SQL 语句），则调用`executeQuery`，如果查询不返回`ResultSet`（如`UPDATE` SQL 语句），则调用`executeUpdate`，如果查询可能返回多个`ResultSet`对象，则调用`execute`。`CoffeesTable.updateCoffeeSales(HashMap<String, Integer>)`中的两个`PreparedStatement`对象都包含`UPDATE` SQL 语句，因此都通过调用`executeUpdate`来执行：

```java
updateSales.setInt(1, e.getValue().intValue());
updateSales.setString(2, e.getKey());
updateSales.executeUpdate();

updateTotal.setInt(1, e.getValue().intValue());
updateTotal.setString(2, e.getKey());
updateTotal.executeUpdate();
con.commit();

```

在执行`updateSales`和`updateTotals`时，`executeUpdate`不需要提供参数；两个`PreparedStatement`对象已经包含要执行的 SQL 语句。

**注意**：在`CoffeesTable.updateCoffeeSales`的开头，自动提交模式被设置为 false：

```java
con.setAutoCommit(false);

```

因此，在调用方法`commit`之前，不会提交任何 SQL 语句。有关自动提交模式的更多信息，请参阅事务。

### executeUpdate 方法的返回值

而`executeQuery`返回一个包含发送到 DBMS 的查询结果的`ResultSet`对象，`executeUpdate`的返回值是一个`int`值，表示更新了表的多少行。例如，下面的代码展示了`executeUpdate`的返回值被赋给变量`n`：

```java
updateSales.setInt(1, 50);
updateSales.setString(2, "Espresso");
int n = updateSales.executeUpdate();
// n = 1 because one row had a change in it

```

表`COFFEES`已更新；值`50`替换了`Espresso`行中`SALES`列的值。这次更新影响了表中的一行，因此`n`等于 1。

当使用方法`executeUpdate`执行 DDL（数据定义语言）语句时，比如创建表时，它会返回值为`int`的 0。因此，在下面的代码片段中，用于执行创建表`COFFEES`的 DDL 语句时，`n`被赋值为 0：

```java
// n = 0
int n = executeUpdate(createTableCoffees); 

```

注意，当`executeUpdate`的返回值为 0 时，可能意味着两种情况之一：

+   执行的语句是一个影响零行的更新语句。

+   执行的语句是一个 DDL 语句。
