# 使用事务

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/transactions.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/transactions.html)

有时候你不希望一个语句生效，除非另一个语句完成。例如，当“咖啡休息时间”的老板更新每周销售的咖啡量时，老板还希望更新迄今为止的总销售量。然而，每周销售量和总销售量应该同时更新；否则，数据将不一致。确保两个动作都发生或两个动作都不发生的方法是使用事务。事务是一组一个或多个语句，作为一个单元执行，因此要么所有语句都执行，要么所有语句都不执行。

本页涵盖以下主题

+   禁用自动提交模式

+   提交事务

+   使用事务保持数据完整性

+   设置和回滚保存点

+   释放保存点

+   何时调用 rollback 方法

## 禁用自动提交模式

当创建连接时，它处于自动提交模式。这意味着每个单独的 SQL 语句都被视为一个事务，并在执行后立即自动提交。（更准确地说，默认情况下，SQL 语句在完成时提交，而不是在执行时。当所有结果集和更新计数都被检索时，语句完成。然而，在几乎所有情况下，语句在执行后立即完成，因此提交。）

允许将两个或多个语句分组为一个事务的方法是禁用自动提交模式。在以下代码中演示了这一点，其中`con`是一个活动连接：

```java
con.setAutoCommit(false);

```

## 提交事务

在禁用自动提交模式后，直到显式调用`commit`方法之前，不会提交任何 SQL 语句。在上一次调用`commit`方法之后执行的所有语句都包含在当前事务中，并作为一个单元一起提交。以下方法`CoffeesTable.updateCoffeeSales`，其中`con`是一个活动连接，演示了一个事务：

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

在这种方法中，连接`con`的自动提交模式被禁用，这意味着当调用`commit`方法时，两个准备好的语句`updateSales`和`updateTotal`将一起提交。每当调用`commit`方法（无论是在自动提交模式启用时自动调用还是在禁用时显式调用），事务中语句导致的所有更改都将变为永久性。在这种情况下，这意味着哥伦比亚咖啡的`SALES`和`TOTAL`列已更改为`50`（如果`TOTAL`之前为`0`），并将保留此值，直到它们通过另一个更新语句进行更改。

语句`con.setAutoCommit(true);`启用自动提交模式，这意味着每个语句再次在完成时自动提交。然后，您回到默认状态，无需自己调用`commit`方法。建议仅在事务模式下禁用自动提交模式。这样，您可以避免为多个语句保持数据库锁，从而增加与其他用户发生冲突的可能性。

## 使用事务保持数据完整性

除了将语句组合在一起作为一个单元执行外，事务还可以帮助保持表中数据的完整性。例如，想象一下，一个员工应该在`COFFEES`表中输入新的咖啡价格，但推迟了几天才这样做。与此同时，价格上涨了，今天所有者正在尝试输入更高的价格。员工最终开始输入现在已过时的价格，与此同时所有者正在尝试更新表。在插入过时价格后，员工意识到这些价格不再有效，并调用`Connection`方法`rollback`来撤消它们的影响。（`rollback`方法中止事务并将值恢复为尝试更新之前的值。）与此同时，所有者正在执行`SELECT`语句并打印新价格。在这种情况下，可能会打印出一个已经回滚到先前值的价格，使打印的价格不正确。

这种情况可以通过使用事务来避免，提供一定程度的保护，防止两个用户同时访问数据时出现冲突。

为了在事务期间避免冲突，数据库管理系统使用锁，这是一种阻止其他人访问事务正在访问的数据的机制。 （请注意，在自动提交模式下，每个语句都是一个事务，锁仅保留一个语句。）设置锁之后，锁将持续有效，直到事务提交或回滚。 例如，数据库管理系统可以锁定表的一行，直到对其进行的更新被提交。 这种锁的效果是防止用户获取脏读取，即在数据永久化之前读取值。 （访问尚未提交的更新值被视为*脏读*，因为可能将该值回滚到其先前的值。 如果读取后来被回滚的值，您将读取一个无效的值。）

锁是如何设置的取决于所谓的事务隔离级别，它可以从根本不支持事务到支持实施非常严格访问规则。

事务隔离级别的一个示例是`TRANSACTION_READ_COMMITTED`，它不允许在提交之后访问值。 换句话说，如果事务隔离级别设置为`TRANSACTION_READ_COMMITTED`，数据库管理系统不允许发生脏读取。 接口`Connection`包括五个值，表示您可以在 JDBC 中使用的事务隔离级别：

| 隔离级别 | 事务 | 脏读 | 不可重复读 | 幻读 |
| --- | --- | --- | --- | --- |
| `TRANSACTION_NONE` | 不支持 | *不适用* | *不适用* | *不适用* |
| `TRANSACTION_READ_COMMITTED` | 支持 | 阻止 | 允许 | 允许 |
| `TRANSACTION_READ_UNCOMMITTED` | 支持 | 允许 | 允许 | 允许 |
| `TRANSACTION_REPEATABLE_READ` | 支持 | 阻止 | 阻止 | 允许 |
| `TRANSACTION_SERIALIZABLE` | 支持 | 阻止 | 阻止 | 阻止 |

当事务 A 检索一行，事务 B 随后更新该行，然后事务 A 稍后再次检索相同的行时，会发生*不可重复读*。 事务 A 两次检索相同的行，但看到不同的数据。

当事务 A 检索满足给定条件的一组行时，事务 B 随后插入或更新一行，使得该行现在满足事务 A 中的条件，然后事务 A 稍后重复条件检索时，会发生*幻读*。 事务 A 现在看到了一个额外的行。 这行被称为幻像。

通常，您不需要对事务隔离级别做任何处理；您可以只使用您的 DBMS 的默认隔离级别。默认事务隔离级别取决于您的 DBMS。例如，对于 Java DB，它是`TRANSACTION_READ_COMMITTED`。JDBC 允许您查找您的 DBMS 设置的事务隔离级别（使用`Connection`方法`getTransactionIsolation`），并且还允许您将其设置为另一个级别（使用`Connection`方法`setTransactionIsolation`）。

**注意**：JDBC 驱动程序可能不支持所有事务隔离级别。如果驱动程序不支持在调用`setTransactionIsolation`时指定的隔离级别，则驱动程序可以替换为更高、更严格的事务隔离级别。如果驱动程序无法替换为更高的事务级别，则会抛出`SQLException`。使用方法`DatabaseMetaData.supportsTransactionIsolationLevel`来确定驱动程序是否支持给定级别。

## 设置和回滚到 Savepoints

方法`Connection.setSavepoint`在当前事务中设置一个`Savepoint`对象。方法`Connection.rollback`被重载以接受一个`Savepoint`参数。

以下方法，`CoffeesTable.modifyPricesByPercentage`，通过百分比`priceModifier`提高特定咖啡的价格。但是，如果新价格大于指定价格`maximumPrice`，则价格将恢复到原始价格：

```java
  public void modifyPricesByPercentage(
    String coffeeName,
    float priceModifier,
    float maximumPrice) throws SQLException {
    con.setAutoCommit(false);
    ResultSet rs = null;
    String priceQuery = "SELECT COF_NAME, PRICE FROM COFFEES " +
                        "WHERE COF_NAME = ?";
    String updateQuery = "UPDATE COFFEES SET PRICE = ? " +
                         "WHERE COF_NAME = ?";
    try (PreparedStatement getPrice = con.prepareStatement(priceQuery, ResultSet.TYPE_SCROLL_INSENSITIVE, ResultSet.CONCUR_READ_ONLY);
         PreparedStatement updatePrice = con.prepareStatement(updateQuery))
    {
      Savepoint save1 = con.setSavepoint();
      getPrice.setString(1, coffeeName);
      if (!getPrice.execute()) {
        System.out.println("Could not find entry for coffee named " + coffeeName);
      } else {
        rs = getPrice.getResultSet();
        rs.first();
        float oldPrice = rs.getFloat("PRICE");
        float newPrice = oldPrice + (oldPrice * priceModifier);
        System.out.printf("Old price of %s is $%.2f%n", coffeeName, oldPrice);
        System.out.printf("New price of %s is $%.2f%n", coffeeName, newPrice);
        System.out.println("Performing update...");
        updatePrice.setFloat(1, newPrice);
        updatePrice.setString(2, coffeeName);
        updatePrice.executeUpdate();
        System.out.println("\nCOFFEES table after update:");
        CoffeesTable.viewTable(con);
        if (newPrice > maximumPrice) {
          System.out.printf("The new price, $%.2f, is greater " +
                            "than the maximum price, $%.2f. " +
                            "Rolling back the transaction...%n",
                            newPrice, maximumPrice);
          con.rollback(save1);
          System.out.println("\nCOFFEES table after rollback:");
          CoffeesTable.viewTable(con);
        }
        con.commit();
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    } finally {
      con.setAutoCommit(true);
    }
  }

```

以下语句指定了从`getPrice`查询生成的`ResultSet`对象的游标在调用`commit`方法时关闭。请注意，如果您的 DBMS 不支持`ResultSet.CLOSE_CURSORS_AT_COMMIT`，则此常量将被忽略：

```java
getPrice = con.prepareStatement(query, ResultSet.CLOSE_CURSORS_AT_COMMIT);

```

该方法首先通过以下语句创建一个`Savepoint`：

```java
Savepoint save1 = con.setSavepoint();

```

该方法检查新价格是否大于`maximumPrice`值。如果是，则使用以下语句回滚事务：

```java
con.rollback(save1);

```

因此，当方法通过调用`Connection.commit`方法提交事务时，它不会提交任何已回滚其关联`Savepoint`的行；它将提交所有其他更新的行。

## 释放 Savepoints

方法`Connection.releaseSavepoint`以`Savepoint`对象作为参数，并将其从当前事务中移除。

释放 Savepoints 后，尝试在回滚操作中引用它会导致抛出`SQLException`。在事务提交或整个事务回滚时，已创建的任何保存点都会自动释放并在事务提交时变为无效，或者在回滚整个事务时变为无效。将事务回滚到保存点会自动释放并使其他在该保存点之后创建的保存点无效。

## 何时调用`rollback`方法

如前所述，调用方法`rollback`会终止事务并将修改的任何值返回到它们之前的值。如果您尝试在事务中执行一个或多个语句并收到`SQLException`，请调用方法`rollback`来结束事务并重新开始事务。这是唯一的方法来知道什么已经提交，什么尚未提交。捕获`SQLException`告诉您出现了问题，但它并不告诉您什么已经提交或尚未提交。因为您不能确定没有任何提交，调用方法`rollback`是确保的唯一方法。

方法`CoffeesTable.updateCoffeeSales`演示了一个事务，并包括一个调用方法`rollback`的`catch`块。如果应用程序继续并使用事务的结果，`catch`块中对`rollback`方法的调用将阻止使用可能不正确的数据。
