# 使用高级数据类型

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/sqltypes.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqltypes.html)

本节介绍的高级数据类型使关系数据库在表列值方面更加灵活。例如，列可以用于存储 `BLOB`（二进制大对象）值，可以以原始字节形式存储非常大量的数据。列也可以是 `CLOB`（字符大对象）类型，能够以字符格式存储非常大量的数据。

ANSI/ISO SQL 标准的最新版本通常被称为 SQL:2003。该标准指定了以下数据类型:

+   SQL92 内置类型，包括熟悉的 SQL 列类型，如 `CHAR`、`FLOAT` 和 `DATE`

+   SQL99 内置类型，包括 SQL99 添加的类型:

    +   `BOOLEAN`: 布尔（真或假）值

    +   `BLOB`: 二进制大对象

    +   `CLOB`: 字符大对象

+   SQL:2003 添加的新内置类型:

    +   `XML`: XML 对象

+   用户定义类型:

    +   结构化类型: 用户定义类型; 例如:

        ```java
        CREATE TYPE PLANE_POINT
        AS (X FLOAT, Y FLOAT) NOT FINAL

        ```

    +   `DISTINCT` 类型: 基于内置类型的用户定义类型; 例如:

        ```java
        CREATE TYPE MONEY
        AS NUMERIC(10,2) FINAL

        ```

+   构造类型: 基于给定基本类型的新类型:

    +   `REF(*structured-type*)`: 持久地指示驻留在数据库中的结构化类型实例的指针

    +   `*base-type* ARRAY[*n*]`: *n* 个基本类型元素的数组

+   定位器: 逻辑指针，指向驻留在数据库服务器上的数据。*定位器* 存在于客户端计算机上，是对服务器上数据的瞬时、逻辑指针。定位器通常指向无法在客户端上具体化的数据，如图像或音频。(*具体化视图* 是事先存储或“具体化”为模式对象的查询结果。) 在 SQL 级别定义了操作符，用于检索由定位器指示的数据的随机访问部分:

    +   `LOCATOR(*structured-type*)`: 服务器中结构化实例的定位器

    +   `LOCATOR(*array*)`: 服务器中数组的定位器

    +   `LOCATOR(*blob*)`: 服务器中二进制大对象的定位器

    +   `LOCATOR(*clob*)`: 服务器中字符大对象的定位器

+   `Datalink`: 用于管理数据源外部数据的类型。`Datalink` 值是 SQL MED（管理外部数据）的一部分，是 SQL ANSI/ISO 标准规范的一部分。

## 映射高级数据类型

JDBC API 为 SQL:2003 标准指定的高级数据类型提供了默认映射。以下列表列出了数据类型及其映射到的接口或类:

+   `BLOB`: `Blob` 接口

+   `CLOB`: `Clob` 接口

+   `NCLOB`: `NClob` 接口

+   `ARRAY`: `Array` 接口

+   `XML`: `SQLXML` 接口

+   结构化类型: `Struct` 接口

+   `REF(structured type)`: `Ref` 接口

+   `ROWID`: `RowId` 接口

+   `DISTINCT`: 基础类型映射的类型。例如，基于 SQL `NUMERIC` 类型的 `DISTINCT` 值映射到 `java.math.BigDecimal` 类型，因为在 Java 编程语言中，`NUMERIC` 映射到 `BigDecimal`。

+   `DATALINK`：`java.net.URL` 对象

## 使用高级数据类型

检索、存储和更新高级数据类型的方式与处理其他数据类型的方式相同。您可以使用 `ResultSet.get*DataType*` 或 `CallableStatement.get*DataType*` 方法来检索它们，`PreparedStatement.set*DataType*` 方法来存储它们，以及 `ResultSet.update*DataType*` 方法来更新它们。（变量 `*DataType*` 是映射到高级数据类型的 Java 接口或类的名称。）大概有 90% 的高级数据类型操作涉及使用 `get*DataType*`、`set*DataType*` 和 `update*DataType*` 方法。以下表格显示了要使用哪些方法：

| **高级数据类型** | **`get*DataType*` 方法** | **`set*DataType*` 方法** | **`update*DataType*` 方法** |
| --- | --- | --- | --- |
| `BLOB` | `getBlob` | `setBlob` | `updateBlob` |
| `CLOB` | `getClob` | `setClob` | `updateClob` |
| `NCLOB` | `getNClob` | `setNClob` | `updateNClob` |
| `ARRAY` | `getArray` | `setArray` | `updateArray` |
| `XML` | `getSQLXML` | `setSQLXML` | `updateSQLXML` |
| `Structured type` | `getObject` | `setObject` | `updateObject` |
| `REF(structured type)` | `getRef` | `setRef` | `updateRef` |
| `ROWID` | `getRowId` | `setRowId` | `updateRowId` |
| `DISTINCT` | `getBigDecimal` | `setBigDecimal` | `updateBigDecimal` |
| `DATALINK` | `getURL` | `setURL` | `updateURL` |

**注意**：`DISTINCT` 数据类型与其他高级 SQL 数据类型的行为不同。作为一个基于已存在内置类型的用户定义类型，它在 Java 编程语言中没有接口。因此，您需要使用与 `DISTINCT` 数据类型基础的 Java 类型对应的方法。请参阅使用 DISTINCT 数据类型获取更多信息。

例如，以下代码片段检索了一个 SQL `ARRAY` 值。在此示例中，假设表 `STUDENTS` 中的列 `SCORES` 包含 `ARRAY` 类型的值。变量 `*stmt*` 是一个 `Statement` 对象。

```java
ResultSet rs = stmt.executeQuery(
    "SELECT SCORES FROM STUDENTS " +
    "WHERE ID = 002238");
rs.next();
Array scores = rs.getArray("SCORES");

```

变量 `*scores*` 是指向存储在表 `STUDENTS` 中学生 `002238` 行中的 SQL `ARRAY` 对象的逻辑指针。

如果要将值存储在数据库中，可以使用相应的 `set` 方法。例如，以下代码片段中，`*rs*` 是一个 `ResultSet` 对象，存储了一个 `Clob` 对象：

```java
Clob notes = rs.getClob("NOTES");
PreparedStatement pstmt =
    con.prepareStatement(
        "UPDATE MARKETS SET COMMENTS = ? " +
        "WHERE SALES < 1000000");
pstmt.setClob(1, notes);
pstmt.executeUpdate();

```

此代码将 `*notes*` 设置为发送到数据库的更新语句的第一个参数。由 `*notes*` 指定的 `Clob` 值将存储在表 `MARKETS` 的 `COMMENTS` 列中，在该列中的值小于一百万的每一行中。
