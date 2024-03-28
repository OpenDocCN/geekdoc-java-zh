# 处理 SQLException

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/sqlexception.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqlexception.html)

本页涵盖以下主题：

+   SQLException 概述

+   检索异常

+   检索警告

+   分类的 SQLException

+   SQLException 的其他子类

## SQLException 概述

当 JDBC 在与数据源交互过程中遇到错误时，会抛出一个`SQLException`实例，而不是`Exception`。（在这种情况下，数据源代表`Connection`对象连接的数据库。）`SQLException`实例包含以下信息，可以帮助您确定错误的原因：

+   错误描述。通过调用方法`SQLException.getMessage`检索包含此描述的`String`对象。

+   SQLState 代码。这些代码及其相应的含义已经由 ISO/ANSI 和 Open Group（X/Open）标准化，尽管一些代码已保留供数据库供应商自行定义。这个`String`对象由五个字母数字字符组成。通过调用方法`SQLException.getSQLState`检索此代码。

+   错误代码。这是一个整数值，标识导致抛出`SQLException`实例的错误。其值和含义是特定于实现的，可能是底层数据源返回的实际错误代码。通过调用方法`SQLException.getErrorCode`检索错误。

+   一个原因。`SQLException`实例可能具有因果关系，由导致抛出`SQLException`实例的一个或多个`Throwable`对象组成。要浏览这些原因链，递归调用方法`SQLException.getCause`，直到返回一个`null`值。

+   任何*链接*异常的引用。如果发生多个错误，这些异常通过此链引用。通过在抛出异常上调用方法`SQLException.getNextException`检索这些异常。

## 检索异常

下面的方法，`JDBCTutorialUtilities.printSQLException`，输出了`SQLException`中包含的 SQLState、错误代码、错误描述以及原因（如果有的话），以及与之链接的任何其他异常：

```java
public static void printSQLException(SQLException ex) {

    for (Throwable e : ex) {
        if (e instanceof SQLException) {
            if (ignoreSQLException(
                ((SQLException)e).
                getSQLState()) == false) {

                e.printStackTrace(System.err);
                System.err.println("SQLState: " +
                    ((SQLException)e).getSQLState());

                System.err.println("Error Code: " +
                    ((SQLException)e).getErrorCode());

                System.err.println("Message: " + e.getMessage());

                Throwable t = ex.getCause();
                while(t != null) {
                    System.out.println("Cause: " + t);
                    t = t.getCause();
                }
            }
        }
    }
}

```

例如，如果您使用 Java DB 调用方法`CoffeesTable.dropTable`，表`COFFEES`不存在，*并且*您删除对`JDBCTutorialUtilities.ignoreSQLException`的调用，输出将类似于以下内容：

```java
SQLState: 42Y55
Error Code: 30000
Message: 'DROP TABLE' cannot be performed on
'TESTDB.COFFEES' because it does not exist.

```

您可以先检索 `SQLState`，然后相应地处理 `SQLException`，而不是打印 `SQLException` 信息。例如，如果 `SQLState` 等于代码 `42Y55`（并且您正在使用 Java DB 作为您的 DBMS），则方法 `JDBCTutorialUtilities.ignoreSQLException` 返回 `true`，导致 `JDBCTutorialUtilities.printSQLException` 忽略 `SQLException`：

```java
public static boolean ignoreSQLException(String sqlState) {

    if (sqlState == null) {
        System.out.println("The SQL state is not defined!");
        return false;
    }

    // X0Y32: Jar file already exists in schema
    if (sqlState.equalsIgnoreCase("X0Y32"))
        return true;

    // 42Y55: Table already exists in schema
    if (sqlState.equalsIgnoreCase("42Y55"))
        return true;

    return false;
}

```

## 检索警告

`SQLWarning` 对象是处理数据库访问警告的 `SQLException` 子类。警告不会像异常那样停止应用程序的执行；它们只是提醒用户某些事情未按计划发生。例如，警告可能会告诉您尝试撤销的权限未被撤销。或者警告可能会告诉您在请求的断开连接期间发生了错误。

可以在 `Connection` 对象、`Statement` 对象（包括 `PreparedStatement` 和 `CallableStatement` 对象）或 `ResultSet` 对象上报告警告。这些类中的每一个都有一个 `getWarnings` 方法，您必须调用它以查看调用对象上报的第一个警告。如果 `getWarnings` 返回一个警告，您可以调用它的 `getNextWarning` 方法来获取任何额外的警告。执行语句会自动清除先前语句的警告，因此它们不会累积。然而，这意味着如果您想要检索在语句上报告的警告，您必须在执行另一个语句之前这样做。

`JDBCTutorialUtilities.java` 中的以下方法演示了如何获取关于 `Statement` 或 `ResultSet` 对象上报的任何警告的完整信息：

```java
public static void getWarningsFromResultSet(ResultSet rs)
    throws SQLException {
    JDBCTutorialUtilities.printWarnings(rs.getWarnings());
}

public static void getWarningsFromStatement(Statement stmt)
    throws SQLException {
    JDBCTutorialUtilities.printWarnings(stmt.getWarnings());
}

public static void printWarnings(SQLWarning warning)
    throws SQLException {

    if (warning != null) {
        System.out.println("\n---Warning---\n");

    while (warning != null) {
        System.out.println("Message: " + warning.getMessage());
        System.out.println("SQLState: " + warning.getSQLState());
        System.out.print("Vendor error code: ");
        System.out.println(warning.getErrorCode());
        System.out.println("");
        warning = warning.getNextWarning();
    }
}

```

最常见的警告是 `DataTruncation` 警告，是 `SQLWarning` 的子类。所有 `DataTruncation` 对象的 SQLState 都是 `01004`，表示读取或写入数据时出现问题。`DataTruncation` 方法让您找出数据在哪一列或参数被截断，截断是在读取还是写入操作中发生的，应该传输多少字节，实际传输了多少字节。

## 分类的 SQLExceptions

您的 JDBC 驱动程序可能会抛出与常见 SQLState 或与特定 SQLState 类值不相关的常见错误状态对应的 `SQLException` 子类。这使您能够编写更具可移植性的错误处理代码。这些异常是以下类的子类之一：

+   `SQLNonTransientException`

+   `SQLTransientException`

+   `SQLRecoverableException`

查看 `java.sql` 包的最新 Javadoc 或您的 JDBC 驱动程序的文档，以获取有关这些子类的更多信息。

## 其他 SQLException 的子类

以下 `SQLException` 的子类也可能被抛出：

+   `BatchUpdateException` 在批量更新操作期间发生错误时抛出。除了`SQLException`提供的信息外，`BatchUpdateException`还提供了在错误发生之前执行的所有语句的更新计数。

+   `SQLClientInfoException` 在连接上无法设置一个或多个客户端信息属性时抛出。除了`SQLException`提供的信息外，`SQLClientInfoException`还提供了未设置的客户端信息属性列表。
