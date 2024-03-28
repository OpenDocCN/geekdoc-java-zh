# 设置表

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/tables.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/tables.html)

本页描述了 JDBC 教程中使用的所有表以及如何创建它们：

+   咖啡表

+   供应商表

+   咖啡库存表

+   商品库存表

+   咖啡店表

+   数据存储库表

+   创建表

+   填充表

## 咖啡表

`COFFEES`表存储了在 The Coffee Break 可供销售的咖啡信息：

| `COF_NAME` | `SUP_ID` | `PRICE` | `SALES` | `TOTAL` |
| --- | --- | --- | --- | --- |
| 哥伦比亚 | 101 | 7.99 | 0 | 0 |
| 法式烘焙 | 49 | 8.99 | 0 | 0 |
| 浓缩咖啡 | 150 | 9.99 | 0 | 0 |
| 哥伦比亚无咖啡因 | 101 | 8.99 | 0 | 0 |
| 法式烘焙无咖啡因 | 49 | 9.99 | 0 | 0 |

以下描述了`COFFEES`表中的每列：

+   `COF_NAME`: 存储咖啡名称。具有 SQL 类型为 `VARCHAR`，最大长度为 32 个字符。由于每种咖啡的名称都不同，因此名称唯一标识特定的咖啡，并作为主键。

+   `SUP_ID`: 存储标识咖啡供应商的数字。具有 SQL 类型为 `INTEGER`。它被定义为引用`SUPPLIERS`表中`SUP_ID`列的外键。因此，DBMS 将强制执行此列中的每个值与`SUPPLIERS`表中相应列中的值之一匹配。

+   `PRICE`: 存储每磅咖啡的成本。具有 SQL 类型为 `FLOAT`，因为它需要存储带有小数点的值。（请注意，货币值通常存储在 SQL 类型 `DECIMAL` 或 `NUMERIC` 中，但由于不同的 DBMS 之间存在差异，并且为了避免与 JDBC 的早期版本不兼容，本教程使用更标准的类型 `FLOAT`。）

+   `SALES`: 存储本周销售的咖啡磅数。具有 SQL 类型为 `INTEGER`。

+   `TOTAL`: 存储迄今为止销售的咖啡磅数。具有 SQL 类型为 `INTEGER`。

## 供应商表

`SUPPLIERS`表存储了每个供应商的信息：

| `SUP_ID` | `SUP_NAME` | `STREET` | `CITY` | `STATE` | `ZIP` |
| --- | --- | --- | --- | --- | --- |
| 101 | Acme, Inc. | 99 Market Street | Groundsville | CA | 95199 |
| 49 | 优质咖啡 | 1 Party Place | Mendocino | CA | 95460 |
| 150 | 高地咖啡 | 100 Coffee Lane | Meadows | CA | 93966 |

以下描述了`SUPPLIERS`表中的每列：

+   `SUP_ID`: 存储标识咖啡供应商的数字。具有 SQL 类型为 `INTEGER`。这是此表中的主键。

+   `SUP_NAME`: 存储咖啡供应商的名称。

+   `STREET`、`CITY`、`STATE` 和 `ZIP`：这些列存储咖啡供应商的地址。

## 咖啡库存表

`COF_INVENTORY`表存储了每个仓库中咖啡的数量信息：

| `WAREHOUSE_ID` | `COF_NAME` | `SUP_ID` | `QUAN` | `DATE_VAL` |
| --- | --- | --- | --- | --- |
| 1234 | House_Blend | 49 | 0 | 2006_04_01 |
| 1234 | House_Blend_Decaf | 49 | 0 | 2006_04_01 |
| 1234 | 哥伦比亚 | 101 | 0 | 2006_04_01 |
| 1234 | 法式烘焙 | 49 | 0 | 2006_04_01 |
| 1234 | 浓缩咖啡 | 150 | 0 | 2006_04_01 |
| 1234 | Colombian_Decaf | 101 | 0 | 2006_04_01 |

以下描述了`COF_INVENTORY`表中每一列的含义：

+   `WAREHOUSE_ID`: 存储标识仓库的数字。

+   `COF_NAME`: 存储特定类型咖啡的名称。

+   `SUP_ID`: 存储标识供应商的数字。

+   `QUAN`: 存储表示商品数量的数字。

+   `DATE`: 存储时间戳数值，表示行最后更新时间。

## `MERCH_INVENTORY`表

`MERCH_INVENTORY`表存储有关库存中非咖啡商品数量的信息：

| `ITEM_ID` | `ITEM_NAME` | `SUP_ID` | `QUAN` | `DATE` |
| --- | --- | --- | --- | --- |
| 00001234 | 大杯 | 00456 | 28 | 2006_04_01 |
| 00001235 | 小杯 | 00456 | 36 | 2006_04_01 |
| 00001236 | 碟子 | 00456 | 64 | 2006_04_01 |
| 00001287 | 咖啡壶 | 00456 | 12 | 2006_04_01 |
| 00006931 | 咖啡壶 | 00927 | 3 | 2006_04_01 |
| 00006935 | 隔热垫 | 00927 | 88 | 2006_04_01 |
| 00006977 | 餐巾纸 | 00927 | 108 | 2006_04_01 |
| 00006979 | 毛巾 | 00927 | 24 | 2006_04_01 |
| 00004488 | 咖啡机 | 08732 | 5 | 2006_04_01 |
| 00004490 | 咖啡研磨机 | 08732 | 9 | 2006_04_01 |
| 00004495 | 咖啡机 | 08732 | 4 | 2006_04_01 |
| 00006914 | 食谱书 | 00927 | 12 | 2006_04_01 |

以下描述了`MERCH_INVENTORY`表中每一列的含义：

+   `ITEM_ID`: 存储标识物品的数字。

+   `ITEM_NAME`: 存储物品的名称。

+   `SUP_ID`: 存储标识供应商的数字。

+   `QUAN`: 存储表示该物品可用数量的数字。

+   `DATE`: 存储时间戳数值，表示行最后更新时间。

## `COFFEE_HOUSES`表

`COFFEE_HOUSES`表存储咖啡店的位置信息：

| `STORE_ID` | `CITY` | `COFFEE` | `MERCH` | `TOTAL` |
| --- | --- | --- | --- | --- |
| 10023 | 门多西诺 | 3450 | 2005 | 5455 |
| 33002 | 西雅图 | 4699 | 3109 | 7808 |
| 10040 | 旧金山 | 5386 | 2841 | 8227 |
| 32001 | 波特兰 | 3147 | 3579 | 6726 |
| 10042 | 旧金山 | 2863 | 1874 | 4710 |
| 10024 | 萨克拉门托 | 1987 | 2341 | 4328 |
| 10039 | 卡梅尔 | 2691 | 1121 | 3812 |
| 10041 | 洛杉矶 | 1533 | 1007 | 2540 |
| 33005 | 奥林匹亚 | 2733 | 1550 | 4283 |
| 33010 | 西雅图 | 3210 | 2177 | 5387 |
| 10035 | 旧金山 | 1922 | 1056 | 2978 |
| 10037 | 洛杉矶 | 2143 | 1876 | 4019 |
| 10034 | 圣何塞 | 1234 | 1032 | 2266 |
| 32004 | Eugene | 1356 | 1112 | 2468 |

以下描述了`COFFEE_HOUSES`表中每一列的含义：

+   `STORE_ID`: 存储标识咖啡店的数字。它表示咖啡店所在州，以 10 开头的值表示加利福尼亚州，以 32 开头的值表示俄勒冈州，以 33 开头的值表示华盛顿州。

+   `CITY`: 存储咖啡馆所在城市的名称。

+   `COFFEE`: 存储表示售出咖啡量的数字。

+   `MERCH`: 存储表示售出商品量的数字。

+   `TOTAL`: 存储表示售出咖啡和商品总量的数字。

## DATA_REPOSITORY 表

表 DATA_REPOSITORY 存储引用文档和其他与 The Coffee Break 有关的数据的 URL。脚本 `populate_tables.sql` 不向此表添加任何数据。以下描述了此表中每列的内容：

+   `DOCUMENT_NAME`: 存储标识 URL 的字符串。

+   `URL`: 存储一个 URL。

## 创建表

您可以使用 Apache Ant 或 JDBC API 创建表。

### 使用 Apache Ant 创建表

要创建教程示例代码中使用的表，请在目录 `*<JDBC tutorial directory>*` 中运行以下命令：

```java
ant setup

```

此命令运行多个 Ant 目标，包括以下内容，`build-tables`（来自 `build.xml` 文件）：

```java
<target name="build-tables"
  description="Create database tables">
  <sql
    driver="${DB.DRIVER}"
    url="${DB.URL}"
    userid="${DB.USER}"
    password="${DB.PASSWORD}"
    classpathref="CLASSPATH"
    delimiter="${DB.DELIMITER}"
    autocommit="false" onerror="abort">
    <transaction src=
  "./sql/${DB.VENDOR}/create-tables.sql"/>
  </sql>
</target>

```

示例为以下 `sql` Ant 任务参数指定值：

| Parameter | 描述 |
| --- | --- |
| `driver` | 您的 JDBC 驱动程序的完全限定类名。此示例中，Java DB 使用 `org.apache.derby.jdbc.EmbeddedDriver`，MySQL Connector/J 使用 `com.mysql.cj.jdbc.Driver`。 |
| `url` | 数据库连接 URL，您的 DBMS JDBC 驱动程序用于连接到数据库的 URL。 |
| `userid` | 您的 DBMS 中有效用户的名称。 |
| `password` | `userid` 中指定用户的密码 |
| `classpathref` | 包含 `driver` 中指定的类的 JAR 文件的完整路径名 |
| `delimiter` | 分隔 SQL 语句的字符串或字符。此示例使用分号 (`;`)。 |
| `autocommit` | 布尔值；如果设置为 `false`，则所有 SQL 语句将作为一个事务执行。 |
| `onerror` | 当语句失败时执行的操作；可能的值为 `continue`、`stop` 和 `abort`。值 `abort` 指定如果发生错误，则事务将被中止。 |

示例将这些参数的值存储在单独的文件中。构建文件 `build.xml` 使用 `import` 任务检索这些值：

```java
<import file="${ANTPROPERTIES}"/>

```

`transaction` 元素指定一个包含要执行的 SQL 语句的文件。文件 `create-tables.sql` 包含创建本页描述的所有表的 SQL 语句。例如，此文件的以下摘录创建表 `SUPPLIERS` 和 `COFFEES`：

```java
create table SUPPLIERS
    (SUP_ID integer NOT NULL,
    SUP_NAME varchar(40) NOT NULL,
    STREET varchar(40) NOT NULL,
    CITY varchar(20) NOT NULL,
    STATE char(2) NOT NULL,
    ZIP char(5),
    PRIMARY KEY (SUP_ID));

create table COFFEES
    (COF_NAME varchar(32) NOT NULL,
    SUP_ID int NOT NULL,
    PRICE numeric(10,2) NOT NULL,
    SALES integer NOT NULL,
    TOTAL integer NOT NULL,
    PRIMARY KEY (COF_NAME),
    FOREIGN KEY (SUP_ID)
        REFERENCES SUPPLIERS (SUP_ID));

```

**注意**：文件 `build.xml` 包含另一个名为 `drop-tables` 的目标，用于删除教程中使用的表。`setup` 目标在运行 `build-tables` 目标之前运行 `drop-tables`。

### 使用 JDBC API 创建表

以下方法，`SuppliersTable.createTable`，创建 `SUPPLIERS` 表：

```java
  public void createTable() throws SQLException {
    String createString =
      "create table SUPPLIERS " + "(SUP_ID integer NOT NULL, " +
      "SUP_NAME varchar(40) NOT NULL, " + "STREET varchar(40) NOT NULL, " +
      "CITY varchar(20) NOT NULL, " + "STATE char(2) NOT NULL, " +
      "ZIP char(5), " + "PRIMARY KEY (SUP_ID))";

    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createString);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

以下方法，`CoffeesTable.createTable`，创建 `COFFEES` 表：

```java
  public void createTable() throws SQLException {
    String createString =
      "create table COFFEES " + "(COF_NAME varchar(32) NOT NULL, " +
      "SUP_ID int NOT NULL, " + "PRICE numeric(10,2) NOT NULL, " +
      "SALES integer NOT NULL, " + "TOTAL integer NOT NULL, " +
      "PRIMARY KEY (COF_NAME), " +
      "FOREIGN KEY (SUP_ID) REFERENCES SUPPLIERS (SUP_ID))";
    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createString);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

在这两种方法中，`con`是一个`Connection`对象，`dbName`是你正在创建表的数据库的名称。

要执行 SQL 查询，比如由`String` `createString`指定的查询，使用一个`Statement`对象。要创建一个`Statement`对象，从现有的`Connection`对象调用方法`Connection.createStatement`。要执行 SQL 查询，调用方法`Statement.executeUpdate`。

所有的`Statement`对象在创建它们的连接关闭时都会被关闭。然而，明确关闭`Statement`对象是良好的编程实践，一旦你完成了它们的使用就立即关闭。这样可以立即释放语句正在使用的任何外部资源。通过调用方法`Statement.close`来关闭一个语句。将这个语句放在`finally`中以确保即使正常程序流被中断，比如抛出异常（比如`SQLException`），它也会关闭。

**注意**：在`COFFEES`之前必须先创建`SUPPLIERS`表，因为`COFFEES`包含一个外键`SUP_ID`，它引用`SUPPLIERS`。

## 填充表

同样，你可以使用 Apache Ant 或 JDBC API 向表中插入数据。

### 使用 Apache Ant 填充表

除了创建本教程使用的表之外，命令`ant setup`还会填充这些表。这个命令运行 Ant 目标`populate-tables`，该目标运行 SQL 脚本`populate-tables.sql`。

下面是从`populate-tables.sql`中填充`SUPPLIERS`和`COFFEES`表的摘录：

```java
insert into SUPPLIERS values(
    49, 'Superior Coffee', '1 Party Place',
    'Mendocino', 'CA', '95460');
insert into SUPPLIERS values(
    101, 'Acme, Inc.', '99 Market Street',
    'Groundsville', 'CA', '95199');
insert into SUPPLIERS values(
    150, 'The High Ground',
    '100 Coffee Lane', 'Meadows', 'CA', '93966');
insert into COFFEES values(
    'Colombian', 00101, 7.99, 0, 0);
insert into COFFEES values(
    'French_Roast', 00049, 8.99, 0, 0);
insert into COFFEES values(
    'Espresso', 00150, 9.99, 0, 0);
insert into COFFEES values(
    'Colombian_Decaf', 00101, 8.99, 0, 0);
insert into COFFEES values(
    'French_Roast_Decaf', 00049, 9.99, 0, 0);

```

### 使用 JDBC API 填充表

下面的方法，`SuppliersTable.populateTable`，将数据插入表中：

```java
  public void populateTable() throws SQLException {
    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate("insert into SUPPLIERS " +
                         "values(49, 'Superior Coffee', '1 Party Place', " +
                         "'Mendocino', 'CA', '95460')");
      stmt.executeUpdate("insert into SUPPLIERS " +
                         "values(101, 'Acme, Inc.', '99 Market Street', " +
                         "'Groundsville', 'CA', '95199')");
      stmt.executeUpdate("insert into SUPPLIERS " +
                         "values(150, 'The High Ground', '100 Coffee Lane', " +
                         "'Meadows', 'CA', '93966')");
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

下面的方法，`CoffeesTable.populateTable`，将数据插入表中：

```java
  public void populateTable() throws SQLException {
    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate("insert into COFFEES " +
                         "values('Colombian', 00101, 7.99, 0, 0)");
      stmt.executeUpdate("insert into COFFEES " +
                         "values('French_Roast', 00049, 8.99, 0, 0)");
      stmt.executeUpdate("insert into COFFEES " +
                         "values('Espresso', 00150, 9.99, 0, 0)");
      stmt.executeUpdate("insert into COFFEES " +
                         "values('Colombian_Decaf', 00101, 8.99, 0, 0)");
      stmt.executeUpdate("insert into COFFEES " +
                         "values('French_Roast_Decaf', 00049, 9.99, 0, 0)");
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```
