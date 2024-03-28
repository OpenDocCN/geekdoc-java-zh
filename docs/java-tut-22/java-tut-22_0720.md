# 从结果集中检索和修改值

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html)

下面的方法，`CoffeesTable.viewTable`，输出了 `COFFEES` 表的内容，并演示了 `ResultSet` 对象和游标的使用：

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

`ResultSet` 对象是表示数据库结果集的数据表，通常是通过执行查询数据库的语句生成的。例如，当 `CoffeeTables.viewTable` 方法通过 `Statement` 对象 `stmt` 执行查询时，会创建一个 `ResultSet` 对象 `rs`。请注意，可以通过实现 `Statement` 接口的任何对象创建 `ResultSet` 对象，包括 `PreparedStatement`、`CallableStatement` 和 `RowSet`。

通过游标访问 `ResultSet` 对象中的数据。请注意，这个游标不是数据库游标。这个游标是指向 `ResultSet` 中一行数据的指针。最初，游标位于第一行之前。方法 `ResultSet.next` 将游标移动到下一行。如果游标位于最后一行之后，则此方法返回 `false`。此方法使用 `while` 循环重复调用 `ResultSet.next` 方法来迭代 `ResultSet` 中的所有数据。

本页涵盖以下主题：

+   ResultSet 接口

+   从行中检索列值

+   游标

+   在 ResultSet 对象中更新行

+   使用 Statement 对象进行批量更新

+   在 ResultSet 对象中插入行

## ResultSet 接口

`ResultSet` 接口提供了检索和操作执行查询结果的方法，`ResultSet` 对象可以具有不同的功能和特性。这些特性包括类型、并发性和游标*保持性*。

### ResultSet 类型

`ResultSet` 对象的类型确定了其功能级别在两个方面：游标如何被操作，以及对底层数据源进行的并发更改如何反映在 `ResultSet` 对象中。

`ResultSet` 对象的灵敏度由三种不同的 `ResultSet` 类型之一确定：

+   `TYPE_FORWARD_ONLY`：结果集无法滚动；其游标仅向前移动，从第一行之前到最后一行之后。结果集中包含的行取决于底层数据库如何生成结果。也就是说，它包含在查询执行时满足查询的行，或者在检索行时满足查询的行。

+   `TYPE_SCROLL_INSENSITIVE`：结果可以滚动；其游标可以相对于当前位置向前和向后移动，并且可以移动到绝对位置。结果集对在打开时对基础数据源进行的更改是不敏感的。它包含在查询执行时满足查询的行，或者在检索行时满足查询的行。

+   `TYPE_SCROLL_SENSITIVE`：结果可以滚动；其游标可以相对于当前位置向前和向后移动，并且可以移动到绝对位置。结果集反映了在结果集保持打开状态时对基础数据源所做的更改。

默认的`ResultSet`类型是`TYPE_FORWARD_ONLY`。

**注意**：并非所有数据库和 JDBC 驱动程序都支持所有`ResultSet`类型。如果指定的`ResultSet`类型受支持，则方法`DatabaseMetaData.supportsResultSetType`返回`true`，否则返回`false`。

### ResultSet 并发性

`ResultSet`对象的并发性确定支持的更新功能级别。

有两个并发级别：

+   `CONCUR_READ_ONLY`：`ResultSet`对象不能使用`ResultSet`接口进行更新。

+   `CONCUR_UPDATABLE`：`ResultSet`对象可以使用`ResultSet`接口进行更新。

默认的`ResultSet`并发性是`CONCUR_READ_ONLY`。

**注意**：并非所有的 JDBC 驱动程序和数据库都支持并发性。如果指定的并发级别由驱动程序支持，则方法`DatabaseMetaData.supportsResultSetConcurrency`返回`true`，否则返回`false`。

方法`CoffeesTable.modifyPrices`演示了如何使用并发级别为`CONCUR_UPDATABLE`的`ResultSet`对象。

### 游标可保持性

调用方法`Connection.commit`可以关闭在当前事务期间创建的`ResultSet`对象。然而，在某些情况下，这可能不是期望的行为。`ResultSet`属性*holdability*使应用程序可以控制在调用`commit`时是否关闭`ResultSet`对象（游标）。

以下`ResultSet`常量可以提供给`Connection`方法`createStatement`，`prepareStatement`和`prepareCall`：

+   `HOLD_CURSORS_OVER_COMMIT`：`ResultSet`游标不会关闭；它们是*可保持的*：当调用`commit`方法时，它们保持打开状态。如果您的应用程序主要使用只读`ResultSet`对象，则可保持的游标可能是理想的。

+   `CLOSE_CURSORS_AT_COMMIT`：在调用`commit`方法时关闭`ResultSet`对象（游标）。在调用此方法时关闭游标可能会提高某些应用程序的性能。

默认的游标可保持性取决于您的 DBMS。

**注意**：并非所有的 JDBC 驱动程序和数据库都支持可保持和不可保持的游标。以下方法`JDBCTutorialUtilities.cursorHoldabilitySupport`输出`ResultSet`对象的默认游标保持性以及是否支持`HOLD_CURSORS_OVER_COMMIT`和`CLOSE_CURSORS_AT_COMMIT`：

```java
public static void cursorHoldabilitySupport(Connection conn)
    throws SQLException {

    DatabaseMetaData dbMetaData = conn.getMetaData();
    System.out.println("ResultSet.HOLD_CURSORS_OVER_COMMIT = " +
        ResultSet.HOLD_CURSORS_OVER_COMMIT);

    System.out.println("ResultSet.CLOSE_CURSORS_AT_COMMIT = " +
        ResultSet.CLOSE_CURSORS_AT_COMMIT);

    System.out.println("Default cursor holdability: " +
        dbMetaData.getResultSetHoldability());

    System.out.println("Supports HOLD_CURSORS_OVER_COMMIT? " +
        dbMetaData.supportsResultSetHoldability(
            ResultSet.HOLD_CURSORS_OVER_COMMIT));

    System.out.println("Supports CLOSE_CURSORS_AT_COMMIT? " +
        dbMetaData.supportsResultSetHoldability(
            ResultSet.CLOSE_CURSORS_AT_COMMIT));
}

```

## 从行中检索列值

`ResultSet`接口声明了获取器方法（例如，`getBoolean`和`getLong`）用于从当前行检索列值。您可以使用列的索引号或别名或名称检索值。列索引通常更有效。列从 1 开始编号。为了最大的可移植性，应按照从左到右的顺序读取每行中的结果集列，并且每列只能读取一次。

例如，以下方法`CoffeesTable.alternateViewTable`，通过编号检索列值：

```java
  public static void alternateViewTable(Connection con) throws SQLException {
    String query = "select COF_NAME, SUP_ID, PRICE, SALES, TOTAL from COFFEES";
    try (Statement stmt = con.createStatement()) {
      ResultSet rs = stmt.executeQuery(query);
      while (rs.next()) {
        String coffeeName = rs.getString(1);
        int supplierID = rs.getInt(2);
        float price = rs.getFloat(3);
        int sales = rs.getInt(4);
        int total = rs.getInt(5);
        System.out.println(coffeeName + ", " + supplierID + ", " + price +
                           ", " + sales + ", " + total);
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

作为获取器方法输入的字符串是不区分大小写的。当使用字符串调用获取器方法时，如果有多个列具有与字符串相同的别名或名称，则返回第一个匹配列的值。使用字符串而不是整数的选项设计用于在生成结果集的 SQL 查询中使用列别名和名称。对于在查询中*未*明确命名的列（例如，`select * from COFFEES`），最好使用列号。如果使用列名，开发人员应确保它们通过使用列别名唯一地引用所需的列。列别名有效地重命名了结果集的列。要指定列别名，请在`SELECT`语句中使用 SQL `AS`子句。

适当类型的获取器方法检索每列中的值。例如，在方法`CoffeeTables.viewTable`中，`ResultSet` `rs`中每行的第一列是`COF_NAME`，存储了 SQL 类型`VARCHAR`的值。检索 SQL 类型`VARCHAR`值的方法是`getString`。每行中的第二列存储了 SQL 类型`INTEGER`的值，检索该类型值的方法是`getInt`。

注意，虽然推荐使用方法`getString`来检索 SQL 类型`CHAR`和`VARCHAR`，但也可以使用它来检索任何基本的 SQL 类型。使用`getString`获取所有值可能非常有用，但也有其局限性。例如，如果用于检索数值类型，`getString`会将数值转换为 Java `String`对象，必须将该值转换回数值类型才能作为数字进行操作。在将值视为字符串处理的情况下，没有任何缺点。此外，如果要求应用程序检索除 SQL3 类型以外的任何标准 SQL 类型的值，请使用`getString`方法。

## 游标

如前所述，通过光标访问`ResultSet`对象中的数据，光标指向`ResultSet`对象中的一行。但是，当创建`ResultSet`对象时，光标位于第一行之前。方法`CoffeeTables.viewTable`通过调用`ResultSet.next`方法移动光标。还有其他可用于移动光标的方法：

+   `next`: 将光标向前移动一行。如果光标现在位于一行上，则返回`true`，如果光标位于最后一行之后，则返回`false`。

+   `previous`: 将光标向后移动一行。如果光标现在位于一行上，则返回`true`，如果光标位于第一行之前，则返回`false`。

+   `first`: 将光标移动到`ResultSet`对象中的第一行。如果光标现在位于第一行上，则返回`true`，如果`ResultSet`对象不包含任何行，则返回`false`。

+   `last:`: 将光标移动到`ResultSet`对象中的最后一行。如果光标现在位于最后一行上，则返回`true`，如果`ResultSet`对象不包含任何行，则返回`false`。

+   `beforeFirst`: 将光标定位在`ResultSet`对象的开头，即第一行之前。如果`ResultSet`对象不包含任何行，则此方法不起作用。

+   `afterLast`: 将光标定位在`ResultSet`对象的末尾，即最后一行之后。如果`ResultSet`对象不包含任何行，则此方法不起作用。

+   `relative(int rows)`: 相对于当前位置移动光标。

+   `absolute(int row)`: 将光标定位在参数`row`指定的行上。

请注意，`ResultSet`的默认灵敏度是`TYPE_FORWARD_ONLY`，这意味着它不能滚动；如果您的`ResultSet`不能滚动，则除了`next`之外，您不能调用任何移动光标的方法。下一节中描述的`CoffeesTable.modifyPrices`方法演示了如何移动`ResultSet`的光标。

## 更新`ResultSet`对象中的行

您不能更新默认的`ResultSet`对象，只能将其光标向前移动。但是，您可以创建可以滚动（光标可以向后移动或移动到绝对位置）和更新的`ResultSet`对象。

以下方法，`CoffeesTable.modifyPrices`，将每行的`PRICE`列乘以参数`percentage`：

```java
  public void modifyPrices(float percentage) throws SQLException {
    try (Statement stmt =
      con.createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_UPDATABLE)) {
      ResultSet uprs = stmt.executeQuery("SELECT * FROM COFFEES");
      while (uprs.next()) {
        float f = uprs.getFloat("PRICE");
        uprs.updateFloat("PRICE", f * percentage);
        uprs.updateRow();
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

字段`ResultSet.TYPE_SCROLL_SENSITIVE`创建一个`ResultSet`对象，其光标可以相对于当前位置和绝对位置向前和向后移动。字段`ResultSet.CONCUR_UPDATABLE`创建一个可更新的`ResultSet`对象。查看`ResultSet` Javadoc 以了解可以指定的其他字段，以修改`ResultSet`对象的行为。

方法`ResultSet.updateFloat`更新了指定列（在本例中为`PRICE`）中光标所在行的指定`float`值。`ResultSet`包含各种更新方法，使您能够更新各种数据类型的列值。然而，这些更新方法都不会修改数据库；您必须调用方法`ResultSet.updateRow`来更新数据库。

## 使用语句对象进行批量更新

`Statement`、`PreparedStatement`和`CallableStatement`对象都有一个与之关联的命令列表。该列表可能包含用于更新、插入或删除行的语句；也可能包含 DDL 语句，如`CREATE TABLE`和`DROP TABLE`。但是，它不能包含会产生`ResultSet`对象的语句，比如`SELECT`语句。换句话说，该列表只能包含产生更新计数的语句。

与`Statement`对象在创建时关联的列表最初为空。您可以使用方法`addBatch`向此列表添加 SQL 命令，并使用方法`clearBatch`清空它。当您完成向列表添加语句时，调用方法`executeBatch`将它们全部发送到数据库以作为一个单元或批量执行。

例如，以下方法，`CoffeesTable.batchUpdate`，使用批量更新向`COFFEES`表添加了四行：

```java
  public void batchUpdate() throws SQLException {
    con.setAutoCommit(false);
    try (Statement stmt = con.createStatement()) {

      stmt.addBatch("INSERT INTO COFFEES " +
                    "VALUES('Amaretto', 49, 9.99, 0, 0)");
      stmt.addBatch("INSERT INTO COFFEES " +
                    "VALUES('Hazelnut', 49, 9.99, 0, 0)");
      stmt.addBatch("INSERT INTO COFFEES " +
                    "VALUES('Amaretto_decaf', 49, 10.99, 0, 0)");
      stmt.addBatch("INSERT INTO COFFEES " +
                    "VALUES('Hazelnut_decaf', 49, 10.99, 0, 0)");

      int[] updateCounts = stmt.executeBatch();
      con.commit();
    } catch (BatchUpdateException b) {
      JDBCTutorialUtilities.printBatchUpdateException(b);
    } catch (SQLException ex) {
      JDBCTutorialUtilities.printSQLException(ex);
    } finally {
      con.setAutoCommit(true);
    }
  }

```

以下行禁用了`Connection`对象`con`的自动提交模式，这样当调用方法`executeBatch`时，事务将不会自动提交或回滚。

```java
con.setAutoCommit(false);

```

为了正确处理错误，您应该在开始批量更新之前始终禁用自动提交模式。

方法`Statement.addBatch`将一个命令添加到与`Statement`对象`stmt`关联的命令列表中。在本例中，这些命令都是`INSERT INTO`语句，每个语句都添加了由五个列值组成的行。列`COF_NAME`和`PRICE`的值分别是咖啡的名称和价格。每行中的第二个值为 49，因为这是供应商 Superior Coffee 的标识号。最后两个值，列`SALES`和`TOTAL`的条目，都从零开始，因为尚未进行销售。（`SALES`是本行咖啡在本周销售的磅数；`TOTAL`是该咖啡所有累计销售的总量。）

以下行将添加到其命令列表中的四个 SQL 命令发送到数据库以作为批量执行：

```java
int[] updateCounts = stmt.executeBatch();

```

请注意，`stmt`使用方法`executeBatch`发送插入的批处理，而不是使用方法`executeUpdate`，后者只发送一个命令并返回单个更新计数。数据库管理系统按照添加到命令列表的顺序执行命令，因此它将首先添加 Amaretto 的值行，然后添加 Hazelnut 的行，然后是 Amaretto decaf，最后是 Hazelnut decaf。如果所有四个命令都成功执行，数据库管理系统将按照执行顺序为每个命令返回一个更新计数。指示每个命令影响了多少行的更新计数存储在数组`updateCounts`中。

如果批处理中的所有四个命令都成功执行，`updateCounts`将包含四个值，所有这些值都为 1，因为插入会影响一行。与`stmt`关联的命令列表现在将为空，因为之前添加的四个命令在`stmt`调用方法`executeBatch`时已发送到数据库。您随时可以使用方法`clearBatch`显式清空此命令列表。

`Connection.commit`方法使对`COFFEES`表的更新批量变为永久。之前为此连接禁用了自动提交模式，因此需要显式调用此方法。

以下行启用当前`Connection`对象的自动提交模式。

```java
con.setAutoCommit(true);

```

现在，示例中的每个语句在执行后将自动提交，并且不再需要调用方法`commit`。

### 执行参数化的批量更新

还可以进行参数化的批量更新，如下面的代码片段所示，其中`con`是一个`Connection`对象：

```java
con.setAutoCommit(false);
PreparedStatement pstmt = con.prepareStatement(
                              "INSERT INTO COFFEES VALUES( " +
                              "?, ?, ?, ?, ?)");
pstmt.setString(1, "Amaretto");
pstmt.setInt(2, 49);
pstmt.setFloat(3, 9.99);
pstmt.setInt(4, 0);
pstmt.setInt(5, 0);
pstmt.addBatch();

pstmt.setString(1, "Hazelnut");
pstmt.setInt(2, 49);
pstmt.setFloat(3, 9.99);
pstmt.setInt(4, 0);
pstmt.setInt(5, 0);
pstmt.addBatch();

// ... and so on for each new
// type of coffee

int[] updateCounts = pstmt.executeBatch();
con.commit();
con.setAutoCommit(true);

```

### 处理批量更新异常

如果批处理中的一个 SQL 语句产生结果集（通常是查询），或者批处理中的一个 SQL 语句由于其他原因未成功执行，当调用方法`executeBatch`时，将会收到`BatchUpdateException`。

你不应该将查询（一个`SELECT`语句）添加到一批 SQL 命令中，因为方法`executeBatch`期望从每个成功执行的 SQL 语句中返回一个更新计数。这意味着只有返回更新计数的命令（如`INSERT INTO`，`UPDATE`，`DELETE`）或返回 0 的命令（如`CREATE TABLE`，`DROP TABLE`，`ALTER TABLE`）才能成功地作为一批与`executeBatch`方法一起执行。

`BatchUpdateException`包含一个与`executeBatch`方法返回的数组类似的更新计数数组。在这两种情况下，更新计数与产生它们的命令的顺序相同。这告诉你批处理中有多少个命令成功执行以及它们是哪些。例如，如果五个命令成功执行，数组将包含五个数字：第一个是第一个命令的更新计数，第二个是第二个命令的更新计数，依此类推。

`BatchUpdateException`是从`SQLException`派生的。这意味着你可以使用所有`SQLException`对象可用的方法。以下方法，`JDBCTutorialUtilities.printBatchUpdateException`，打印了所有`SQLException`信息以及`BatchUpdateException`对象中包含的更新计数。因为`BatchUpdateException.getUpdateCounts`返回一个`int`数组，代码使用`for`循环打印每个更新计数：

```java
  public static void printBatchUpdateException(BatchUpdateException b) {
    System.err.println("----BatchUpdateException----");
    System.err.println("SQLState:  " + b.getSQLState());
    System.err.println("Message:  " + b.getMessage());
    System.err.println("Vendor:  " + b.getErrorCode());
    System.err.print("Update counts:  ");
    int[] updateCounts = b.getUpdateCounts();
    for (int i = 0; i < updateCounts.length; i++) {
      System.err.print(updateCounts[i] + "   ");
    }
  }

```

## 在`ResultSet`对象中插入行

**注意**：并非所有的 JDBC 驱动程序都支持使用`ResultSet`接口插入新行。如果尝试插入新行而你的 JDBC 驱动程序数据库不支持此功能，将抛出`SQLFeatureNotSupportedException`异常。

以下方法，`CoffeesTable.insertRow`，通过`ResultSet`对象向`COFFEES`插入一行：

```java
  public void insertRow(String coffeeName, int supplierID, float price,
                        int sales, int total) throws SQLException {

    try (Statement stmt =
          con.createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_UPDATABLE))
    {      
      ResultSet uprs = stmt.executeQuery("SELECT * FROM COFFEES");
      uprs.moveToInsertRow();
      uprs.updateString("COF_NAME", coffeeName);
      uprs.updateInt("SUP_ID", supplierID);
      uprs.updateFloat("PRICE", price);
      uprs.updateInt("SALES", sales);
      uprs.updateInt("TOTAL", total);

      uprs.insertRow();
      uprs.beforeFirst();

    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

此示例调用`Connection.createStatement`方法，带有两个参数，`ResultSet.TYPE_SCROLL_SENSITIVE`和`ResultSet.CONCUR_UPDATABLE`。第一个值使`ResultSet`对象的游标能够向前和向后移动。第二个值，`ResultSet.CONCUR_UPDATABLE`，如果要向`ResultSet`对象插入行，则需要它；它指定它可以被更新。

在 getter 方法中使用字符串的相同规定也适用于 updater 方法。

方法`ResultSet.moveToInsertRow`将游标移动到插入行。插入行是与可更新结果集关联的特殊行。它本质上是一个缓冲区，可以通过调用更新器方法构造新行，然后将该行插入结果集。例如，此方法调用`ResultSet.updateString`方法将插入行的`COF_NAME`列更新为`Kona`。

方法`ResultSet.insertRow`将插入行的内容插入`ResultSet`对象和数据库中。

**注意**：使用`ResultSet.insertRow`插入一行后，应将游标移动到插入行之外的行。例如，此示例使用`ResultSet.beforeFirst`方法将其移动到结果集中的第一行之前。如果应用程序的另一部分使用相同的结果集且游标仍指向插入行，则可能会出现意外结果。
