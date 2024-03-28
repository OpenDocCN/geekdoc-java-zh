# 使用数组对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/array.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/array.html)

**注意**：MySQL 和 Java DB 目前不支持`ARRAY` SQL 数据类型。因此，没有可用的 JDBC 教程示例来演示`Array` JDBC 数据类型。

下面的主题包括：

+   创建数组对象

+   检索和访问 ResultSet 中的数组值

+   存储和更新数组对象

+   释放数组资源

## 创建数组对象

使用方法`Connection.createArrayOf`创建`Array`对象。

例如，假设您的数据库包含一个名为`REGIONS`的表，该表已经通过以下 SQL 语句创建并填充；请注意，这些语句的语法将根据您的数据库而变化：

```java
create table REGIONS
    (REGION_NAME varchar(32) NOT NULL,
    ZIPS varchar32 ARRAY[10] NOT NULL,
    PRIMARY KEY (REGION_NAME));

insert into REGIONS values(
    'Northwest',
    '{"93101", "97201", "99210"}');
insert into REGIONS values(
    'Southwest',
    '{"94105", "90049", "92027"}');

```

```java
Connection con = DriverManager.getConnection(url, props);
String [] northEastRegion = { "10022", "02110", "07399" };
Array anArray = con.createArrayOf("VARCHAR", northEastRegion);

```

Oracle 数据库 JDBC 驱动程序使用`oracle.sql.ARRAY`类实现`java.sql.Array`接口。

## 在 ResultSet 中检索和访问数组值

与 JDBC 4.0 大对象接口（`Blob`，`Clob`，`NClob`）一样，您可以操作`Array`对象，而无需将所有数据从数据库服务器传输到客户端计算机。`Array`对象将其表示的 SQL `ARRAY`作为结果集或 Java 数组实现。

以下摘录检索列`ZIPS`中的 SQL `ARRAY`值，并将其赋给`java.sql.Array`对象`z`对象。摘录检索`z`的内容并将其存储在`zips`中，`zips`是一个包含`String`类型对象的 Java 数组。摘录遍历`zips`数组并检查每个邮政（邮编）代码是否有效。此代码假定类`ZipCode`已经在先前定义，并且具有方法`isValid`，如果给定的邮政编码与有效邮政编码主列表中的一个匹配，则返回`true`：

```java
ResultSet rs = stmt.executeQuery(
    "SELECT region_name, zips FROM REGIONS");

while (rs.next()) {
    Array z = rs.getArray("ZIPS");
    String[] zips = (String[])z.getArray();
    for (int i = 0; i < zips.length; i++) {
        if (!ZipCode.isValid(zips[i])) {
            // ...
            // Code to display warning
        }
    }
}

```

在以下语句中，`ResultSet`方法`getArray`将当前行的列`ZIPS`中存储的值作为`java.sql.Array`对象`z`返回：

```java
Array z = rs.getArray("ZIPS");

```

变量`*z*`包含一个定位器，这是指向服务器上 SQL `ARRAY`的逻辑指针；它不包含`ARRAY`本身的元素。作为逻辑指针，`*z*`可用于在服务器上操作数组。

在以下行中，`getArray`是`Array.getArray`方法，而不是前一行中使用的`ResultSet.getArray`方法。因为`Array.getArray`方法在 Java 编程语言中返回一个`Object`，并且每个邮政编码都是一个`String`对象，所以在分配给变量`zips`之前，结果被转换为`String`对象的数组。

```java
String[] zips = (String[])z.getArray();

```

`Array.getArray`方法将 SQL `ARRAY`元素在客户端作为`String`对象数组实现。因为实际上变量`*zips*`包含数组的元素，所以可以在`for`循环中遍历`zips`，查找无效的邮政编码。

## 存储和更新数组对象

使用方法`PreparedStatement.setArray`和`PreparedStatement.setObject`将`Array`值作为输入参数传递给`PreparedStatement`对象。

以下示例将在先前创建的`Array`对象`anArray`设置为`pstmt`的第二个参数：

```java
PreparedStatement pstmt = con.prepareStatement(
    "insert into REGIONS (region_name, zips) " + "VALUES (?, ?)");
pstmt.setString(1, "NorthEast");
pstmt.setArray(2, anArray);
pstmt.executeUpdate();

```

同样，使用方法`PreparedStatement.updateArray`和`PreparedStatement.updateObject`来使用`Array`值更新表中的列。

## 释放数组资源

`Array`对象在创建它们的事务持续时间内保持有效。这可能导致应用程序在长时间运行的事务中耗尽资源。应用程序可以通过调用它们的`free`方法来释放`Array`资源。

在以下摘录中，调用方法`Array.free`来释放先前创建的`Array`对象所持有的资源。

```java
Array aArray = con.createArrayOf("VARCHAR", northEastRegionnewYork);
// ...
aArray.free();

```
