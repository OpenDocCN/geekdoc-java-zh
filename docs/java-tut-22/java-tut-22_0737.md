# 使用 RowId 对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/sqlrowid.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqlrowid.html)

**注意**：MySQL 和 Java DB 目前不支持`RowId` JDBC 接口。因此，没有可用的 JDBC 教程示例来演示本节中描述的功能。

`RowId`对象表示数据库表中一行的地址。但请注意，`ROWID`类型不是标准 SQL 类型。`ROWID`值可能很有用，因为它们通常是访问单个行的最快方式，并且是表中行的唯一标识符。但是，您不应将`ROWID`值用作表的主键。例如，如果从表中删除特定行，则数据库可能会将其`ROWID`值重新分配给稍后插入的行。

下面涵盖了以下主题：

+   检索 RowId 对象

+   使用 RowId 对象

+   RowId 有效期

## 检索 RowId 对象

通过调用接口`ResultSet`和`CallableStatement`中定义的 getter 方法检索`java.sql.RowId`对象。返回的`RowId`对象是一个不可变对象，您可以将其用作后续引用的唯一标识符。以下是调用`ResultSet.getRowId`方法的示例：

```java
java.sql.RowId rowId_1 = rs.getRowId(1);

```

## 使用 RowId 对象

您可以将`RowId`对象设置为参数传递给参数化的`PreparedStatement`对象：

```java
Connection conn = ds.getConnection(username, password);
PreparedStatement ps = conn.prepareStatement(
    "INSERT INTO BOOKLIST" +
    "(ID, AUTHOR, TITLE, ISBN) " +
    "VALUES (?, ?, ?, ?)");
ps.setRowId(1, rowId_1);

```

您还可以在可更新的`ResultSet`对象中使用特定的`RowId`对象更新列：

```java
ResultSet rs = ...
rs.next();
rs.updateRowId(1, rowId_1);

```

`RowId`对象的值通常在数据源之间不可移植，并且在分别使用`PreparedStatement`和`ResultSet`对象的设置或更新方法时应被视为特定于数据源。因此，不建议从连接到一个数据源的`ResultSet`对象获取`RowId`对象，然后尝试在连接到不同数据源的不相关`ResultSet`对象中使用相同的`RowId`对象。

## RowId 有效期

只要识别的行未被删除，`RowId`对象就有效，并且`RowId`对象的生命周期在数据源为`RowId`指定的生命周期范围内。

要确定数据库或数据源中`RowId`对象的生命周期，请调用方法`DatabaseMetaData.getRowIdLifetime`。它返回一个`RowIdLifetime`枚举数据类型的值。以下方法，`JDBCTutorialUtilities.rowIdLifeTime`，返回`RowId`对象的生命周期：

```java
public static void rowIdLifetime(Connection conn)
    throws SQLException {

    DatabaseMetaData dbMetaData = conn.getMetaData();
    RowIdLifetime lifetime = dbMetaData.getRowIdLifetime();

    switch (lifetime) {
        case ROWID_UNSUPPORTED:
            System.out.println("ROWID type not supported");
            break;

        case ROWID_VALID_FOREVER:
            System.out.println("ROWID has unlimited lifetime");
            break;

        case ROWID_VALID_OTHER:
            System.out.println("ROWID has indeterminate lifetime");
            break;

        case ROWID_VALID_SESSION:
            System.out.println(
                "ROWID type has lifetime that " +
                "is valid for at least the " +
                "containing session");
            break;

        case ROWID_VALID_TRANSACTION:
            System.out.println(
                "ROWID type has lifetime that " +
                "is valid for at least the " +
                "containing transaction");
            break;
    }
}

```
