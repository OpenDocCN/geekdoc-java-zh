# 使用存储过程

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/storedprocedures.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/storedprocedures.html)

存储过程是一组 SQL 语句，形成一个逻辑单元并执行特定任务，它们用于封装一组操作或查询以在数据库服务器上执行。例如，对员工数据库的操作（雇佣、解雇、晋升、查找）可以编码为应用程序代码执行的存储过程。存储过程可以编译并使用不同的参数和结果执行，并且可以具有任何组合的输入、输出和输入/输出参数。

请注意，大多数 DBMS 都支持存储过程，但它们的语法和功能有相当多的变化。因此，本教程包含两个示例，`StoredProcedureJavaDBSample.java` 和 `StoredProcedureMySQLSample.java`，分别演示如何在 Java DB 和 MySQL 中创建存储过程。

本页涵盖以下主题：

+   存储过程示例概述

+   参数模式

+   在 Java DB 中创建存储过程

    +   使用 SQL 脚本或 JDBC API 在 Java DB 中创建存储过程

    +   使用 SQL 脚本或 JDBC API 在 Java DB 中创建存储过程

    +   在 Java DB 中调用存储过程

    +   将 Java 类打包到 JAR 文件中

+   在 MySQL 中创建存储过程

    +   使用 SQL 脚本或 JDBC API 在 MySQL 中创建存储过程

    +   在 MySQL 中调用存储过程

## 存储过程示例概述

示例`StoredProcedureJavaDBSample.java` 和 `StoredProcedureMySQLSample.java` 创建并调用以下存储过程：

+   `SHOW_SUPPLIERS`: 打印一个包含咖啡供应商名称和他们向 The Coffee Break 供应的咖啡的结果集。此存储过程不需要任何参数。当示例调用此存储过程时，示例产生类似以下内容的输出：

    ```java
    Acme, Inc.: Colombian_Decaf
    Acme, Inc.: Colombian
    Superior Coffee: French_Roast_Decaf
    Superior Coffee: French_Roast
    The High Ground: Espresso

    ```

+   `GET_SUPPLIER_OF_COFFEE`: 打印供应商`supplierName`为咖啡`coffeeName`的名称。它需要以下参数：

    +   `IN coffeeName varchar(32)`: 咖啡的名称

    +   `OUT supplierName varchar(40)`: 咖啡供应商的名称

    当示例以`Colombian`作为`coffeeName`的值调用此存储过程时，示例产生类似以下内容的输出：

    ```java
    Supplier of the coffee Colombian: Acme, Inc.

    ```

+   `RAISE_PRICE`：将咖啡`coffeeName`的价格提高到价格`newPrice`。如果价格增加大于百分比`maximumPercentage`，则价格将按该百分比提高。如果价格`newPrice`低于咖啡的原始价格，则此过程不会更改价格。它需要以下参数：

    +   `IN coffeeName varchar(32)`：咖啡的名称

    +   `IN maximumPercentage float`：提高咖啡价格的最大百分比

    +   `INOUT newPrice numeric(10,2)`：咖啡的新价格。调用`RAISE_PRICE`存储过程后，此参数将包含咖啡`coffeeName`的当前价格。

    当示例以`Colombian`作为`coffeeName`的值，`0.10`作为`maximumPercentage`的值，`19.99`作为`newPrice`的值调用此存储过程时，示例会产生类似以下输出：

    ```java
    Contents of COFFEES table before calling RAISE_PRICE:
    Colombian, 101, 7.99, 0, 0
    Colombian_Decaf, 101, 8.99, 0, 0
    Espresso, 150, 9.99, 0, 0
    French_Roast, 49, 8.99, 0, 0
    French_Roast_Decaf, 49, 9.99, 0, 0

    Calling the procedure RAISE_PRICE

    Value of newPrice after calling RAISE_PRICE: 8.79

    Contents of COFFEES table after calling RAISE_PRICE:
    Colombian, 101, 8.79, 0, 0
    Colombian_Decaf, 101, 8.99, 0, 0
    Espresso, 150, 9.99, 0, 0
    French_Roast, 49, 8.99, 0, 0
    French_Roast_Decaf, 49, 9.99, 0, 0

    ```

## 参数模式

参数属性`IN`（默认值）、`OUT`和`INOUT`是参数模式。它们定义形式参数的操作。以下表总结了有关参数模式的信息。

| 参数模式的特征 | IN | OUT | INOUT |
| --- | --- | --- | --- |
| 必须在存储过程定义中指定吗？ | 不需要；如果省略，则形式参数的参数模式为`IN`。 | 必须指定。 | 必须指定。 |
| 参数是否向存储过程传递值或返回值？ | 向存储过程传递值。 | 返回值给调用者。 | 两者都是；向存储过程传递初始值；返回更新后的值给调用者。 |
| 形式参数在存储过程中是作为常量还是变量？ | 形式参数像常量一样起作用。 | 形式参数像未初始化的变量一样起作用。 | 形式参数像初始化的变量一样起作用。 |
| 形式参数是否可以在存储过程中分配值？ | 形式参数不能分配值。 | 形式参数不能在表达式中使用；必须分配值。 | 形式参数必须分配值。 |
| 可以传递给存储过程的实际参数（参数）有哪些？ | 实际参数可以是常量、初始化变量、文字常量或表达式。 | 实际参数必须是一个变量。 | 实际参数必须是一个变量。 |

## 在 Java DB 中创建存储过程

**注意**：有关在 Java DB 中创建存储过程的更多信息，请参阅[*Java DB 参考手册*](https://docs.oracle.com/javadb/index_jdk8.html)中的“CREATE PROCEDURE 语句”部分。

在 Java DB 中创建和使用存储过程涉及以下步骤：

1.  在 Java 类中创建一个公共静态 Java 方法：此方法执行存储过程所需的任务。

1.  创建存储过程：此存储过程调用您创建的 Java 方法。

1.  调用存储过程

1.  将包含您之前创建的公共静态 Java 方法的 Java 类打包到一个 JAR 文件中。

### 创建公共静态 Java 方法

下面的方法，`StoredProcedureJavaDBSample.showSuppliers`，包含存储过程`SHOW_SUPPLIERS`调用的 SQL 语句：

```java
public static void showSuppliers(ResultSet[] rs)
    throws SQLException {

    Connection con = DriverManager.getConnection("jdbc:default:connection");
    Statement stmt = null;

    String query =
        "select SUPPLIERS.SUP_NAME, " +
        "COFFEES.COF_NAME " +
        "from SUPPLIERS, COFFEES " +
        "where SUPPLIERS.SUP_ID = " +
        "COFFEES.SUP_ID " +
        "order by SUP_NAME";

    stmt = con.createStatement();
    rs[0] = stmt.executeQuery(query);
}

```

`SHOW_SUPPLIERS`存储过程不接受任何参数。您可以通过在公共静态 Java 方法的方法签名中定义参数来指定存储过程中的参数。请注意，方法`showSuppliers`包含一个类型为`ResultSet[]`的参数。如果您的存储过程返回任意数量的`ResultSet`对象，请在您的 Java 方法中指定一个类型为`ResultSet[]`的参数。此外，请确保此 Java 方法是公共的和静态的。

从 URL `jdbc:default:connection` 中检索`Connection`对象。这是 Java DB 中的一种约定，表示存储过程将使用当前存在的`Connection`对象。

请注意，在此方法中未关闭`Statement`对象。不要在存储过程的 Java 方法中关闭任何`Statement`对象；如果这样做，当您调用存储过程时，`ResultSet`对象将不存在。

为了使存储过程返回一个生成的结果集，您必须将结果集分配给`ResultSet[]`参数的一个数组组件。在本例中，生成的结果集分配给了数组组件`rs[0]`。

### 使用 SQL 脚本或 JDBC API 在 Java DB 中创建存储过程

Java DB 使用 Java 编程语言进行存储过程。因此，当您定义存储过程时，您需要指定要调用的 Java 类以及 Java DB 可以找到它的位置。

以下摘录自`StoredProcedureJavaDBSample.createProcedures`创建了一个名为`SHOW_SUPPLIERS`的存储过程：

```java
public void createProcedures(Connection con)
    throws SQLException {

    Statement stmtCreateShowSuppliers = null;

    // ...

    String queryShowSuppliers =
        "CREATE PROCEDURE SHOW_SUPPLIERS() " +
        "PARAMETER STYLE JAVA " +
        "LANGUAGE JAVA " +
        "DYNAMIC RESULT SETS 1 " +
        "EXTERNAL NAME " +
        "'com.oracle.tutorial.jdbc." +
        "StoredProcedureJavaDBSample." +
        "showSuppliers'";

    // ...

    try {
        System.out.println("Calling CREATE PROCEDURE");
        stmtCreateShowSuppliers = con.createStatement();

        // ...

    } catch (SQLException e) {
        JDBCTutorialUtilities.printSQLException(e);
    } finally {
        if (stmtCreateShowSuppliers != null) {
            stmtCreateShowSuppliers.close();
        }
        // ...
    }
}

```

以下列表描述了您可以在`CREATE PROCEDURE`语句中指定的过程元素：

+   `PARAMETER STYLE`：标识用于将参数传递给存储过程的约定。以下选项有效：

    +   `JAVA`：指定存储过程使用符合 Java 语言和 SQL 例程规范的参数传递约定。

    +   `DERBY`：指定存储过程支持参数列表中的最后一个参数作为可变参数。

+   `LANGUAGE JAVA`：指定存储过程的编程语言（目前，`JAVA`是唯一的选项）。

+   `DYNAMIC RESULT SETS 1`：指定检索的最大结果集数量；在本例中为`1`。

+   `EXTERNAL NAME 'com.oracle.tutorial.jdbc.StoredProcedureJavaDBSample.showSuppliers'` 指定了此存储过程调用的完全限定的 Java 方法。**注意**：Java DB 必须能够在类路径或直接添加到数据库的 JAR 文件中找到此处指定的方法。请参阅以下步骤，将 Java 类打包到 JAR 文件中。

以下语句（位于 `StoredProcedureJavaDBSample.createProcedures` 中）创建了一个名为 `GET_SUPPLIERS_OF_COFFEE` 的存储过程（为了清晰起见添加了换行符）：

```java
CREATE PROCEDURE GET_SUPPLIER_OF_COFFEE(
    IN coffeeName varchar(32),
    OUT supplierName
    varchar(40))
    PARAMETER STYLE JAVA
    LANGUAGE JAVA
    DYNAMIC RESULT SETS 0
    EXTERNAL NAME 'com.oracle.tutorial.jdbc.
        StoredProcedureJavaDBSample.
        getSupplierOfCoffee'

```

此存储过程有两个形式参数，`coffeeName` 和 `supplierName`。参数说明符 `IN` 和 `OUT` 被称为参数模式。它们定义了形式参数的操作。有关更多信息，请参阅参数模式。此存储过程不检索结果集，因此过程元素 `DYNAMIC RESULT SETS` 为 `0`。

以下语句创建了一个名为 `RAISE_PRICE` 的存储过程（为了清晰起见添加了换行符）：

```java
CREATE PROCEDURE RAISE_PRICE(
    IN coffeeName varchar(32),
    IN maximumPercentage float,
    INOUT newPrice float)
    PARAMETER STYLE JAVA
    LANGUAGE JAVA
    DYNAMIC RESULT SETS 0
    EXTERNAL NAME 'com.oracle.tutorial.jdbc.
        StoredProcedureJavaDBSample.raisePrice'

```

您可以使用 SQL 脚本在 Java DB 中创建存储过程。查看脚本 `javadb/create-procedures.sql` 和 `build.xml` Ant 构建脚本中的 Ant 目标 `javadb-create-procedure`。

### 在 Java DB 中调用存储过程

以下摘录自方法`StoredProcedureJavaDBSample.runStoredProcedures` 调用存储过程 `SHOW_SUPPLIERS` 并打印生成的结果集：

```java
cs = this.con.prepareCall("{call SHOW_SUPPLIERS()}");
ResultSet rs = cs.executeQuery();

while (rs.next()) {
    String supplier = rs.getString("SUP_NAME");
    String coffee = rs.getString("COF_NAME");
    System.out.println(supplier + ": " + coffee);
}

```

**注意**：与 `Statement` 对象一样，要调用存储过程，可以根据过程返回多少个 `ResultSet` 对象来调用 `execute`、`executeQuery` 或 `executeUpdate`。但是，如果不确定过程返回多少个 `ResultSet` 对象，请调用 `execute`。

以下摘录自方法 `StoredProcedureJavaDBSample.runStoredProcedures` 调用存储过程 `GET_SUPPLIER_OF_COFFEE`：

```java
cs = this.con.prepareCall("{call GET_SUPPLIER_OF_COFFEE(?, ?)}");
cs.setString(1, coffeeNameArg);
cs.registerOutParameter(2, Types.VARCHAR);
cs.executeQuery();

String supplierName = cs.getString(2);

```

接口 `CallableStatement` 扩展了 `PreparedStatement`。它用于调用存储过程。像使用 `PreparedStatement` 对象一样，通过调用适当的 setter 方法为 `IN` 参数（例如本例中的 `coffeeName`）指定值。但是，如果存储过程包含 `OUT` 参数，则必须使用 `registerOutParameter` 方法进行注册。

以下摘录自方法 `StoredProcedureJavaDBSample.runStoredProcedures` 调用存储过程 `RAISE_PRICE`：

```java
cs = this.con.prepareCall("{call RAISE_PRICE(?,?,?)}");
cs.setString(1, coffeeNameArg);
cs.setFloat(2, maximumPercentageArg);
cs.registerOutParameter(3, Types.NUMERIC);
cs.setFloat(3, newPriceArg);

cs.execute();

```

因为参数 `newPrice`（过程 `RAISE_PRICE` 中的第三个参数）具有参数模式 `INOUT`，您必须通过调用适当的 setter 方法指定其值，并使用 `registerOutParameter` 方法进行注册。

### 将 Java 类打包到 JAR 文件中

Ant 构建脚本 `build.xml` 包含编译和打包教程为 JAR 文件的目标。在命令提示符下，将当前目录更改为 `*<JDBC tutorial directory>*`。从该目录运行以下命令编译并打包教程为 JAR 文件：

`ant jar`

JAR 文件的名称是 `*<JDBC tutorial directory>*/lib/JDBCTutorial.jar`。

Ant 构建脚本将文件 `JDBCTutorial.jar` 添加到类路径中。您还可以在 `CLASSPATH` 环境变量中指定 JAR 文件的位置。这样可以使 Java DB 找到存储过程调用的 Java 方法。

#### 直接向数据库添加 JAR 文件

Java DB 首先在类路径中查找所需的类，然后在数据库中查找。本节展示了如何直接向数据库添加 JAR 文件。

使用以下系统存储过程将 `JDBCTutorial.jar` JAR 文件添加到数据库中（为了清晰起见已添加换行符）：

```java
CALL sqlj.install_jar(
  '*<JDBC tutorial directory>*/
  lib/JDBCTutorial.jar',
  'APP.JDBCTutorial', 0)
CALL sqlj.replace_jar(
  '*<JDBC tutorial directory>*/
  lib/JDBCTutorial.jar',
  'APP.JDBCTutorial')";
CALL syscs_util.syscs_set_database_property(
  'derby.database.classpath',
  'APP.JDBCTutorial')";

```

**注意**：方法 `StoredProcedureJavaDBSample.registerJarFile` 演示了如何调用这些系统存储过程。如果调用此方法，请确保已修改 `javadb-sample-properties.xml`，使属性 `jar_file` 的值设置为 `JDBCTutorial.jar` 的完整路径名。

`SQL` 模式中的 `install_jar` 过程向数据库添加 JAR 文件。此过程的第一个参数是在运行此过程的计算机上 JAR 文件的完整路径名。第二个参数是 Java DB 用于引用 JAR 文件的标识符。（标识符 `APP` 是 Java DB 默认模式。）`replace_jar` 过程替换数据库中已有的 JAR 文件。

系统存储过程 `SYSCS_UTIL.SYSCS_SET_DATABASE_PROPERTY` 在当前连接上设置或删除数据库属性的值。此方法将属性 `derby.database.classpath` 设置为 `install_jar` 文件中指定的标识符。Java DB 首先在 Java 类路径中查找类，然后查找 `derby.database.classpath`。

## 在 MySQL 中创建存储过程

在 Java DB 中创建和使用存储过程涉及以下步骤：

1.  使用 SQL 脚本或 JDBC API 创建存储过程。

1.  使用 `CALL` SQL 语句调用存储过程。参见 在 MySQL 中调用存储过程 部分。

### 使用 SQL 脚本或 JDBC API 在 MySQL 中创建存储过程

MySQL 使用基于 SQL 的语法来编写存储过程。以下摘录来自 SQL 脚本 `mysql/create-procedures.sql` 创建了名为 `SHOW_SUPPLIERS` 的存储过程：

```java
SELECT 'Dropping procedure SHOW_SUPPLIERS' AS ' '|
drop procedure if exists SHOW_SUPPLIERS|

# ...

SELECT 'Creating procedure SHOW_SUPPLIERS' AS ' '|
create procedure SHOW_SUPPLIERS()
    begin
        select SUPPLIERS.SUP_NAME,
        COFFEES.COF_NAME
        from SUPPLIERS, COFFEES
        where SUPPLIERS.SUP_ID = COFFEES.SUP_ID
        order by SUP_NAME;
    end|

```

`DROP PROCEDURE` 语句会删除存储过程 `SHOW_SUPPLIERS`（如果存在的话）。在 MySQL 中，存储过程中的语句用分号分隔。然而，结束 `create procedure` 语句需要一个不同的分隔符。这个示例使用了竖线（`|`）字符；你可以使用其他字符（或多个字符）。分隔语句的字符在调用这个脚本的 Ant 目标中的 `delimiter` 属性中定义。这段摘录来自 Ant 构建文件 `build.xml`（为了清晰起见插入了换行符）：

```java
<target name="mysql-create-procedure">

  <sql driver="${DB.DRIVER}"
       url="${DB.URL}" userid="${DB.USER}"
       password="${DB.PASSWORD}"
       classpathref="CLASSPATH"
       print="true"
       delimiter="|"
       autocommit="false"
       onerror="abort">
       <transaction
         src="./sql/${DB.VENDOR}/
           create-procedures.sql">
       </transaction>
  </sql>

</target>

```

或者，你可以使用 `DELIMITER` SQL 语句来指定一个不同的分隔符字符。

`CREATE PROCEDURE` 语句由过程的名称、括号中以逗号分隔的参数列表以及 `BEGIN` 和 `END` 关键字内的 SQL 语句组成。

你可以使用 JDBC API 来创建存储过程。下面的方法 `StoredProcedureMySQLSample.createProcedureShowSuppliers` 执行了与前面脚本相同的任务：

```java
  public void createProcedureShowSuppliers() throws SQLException {

    String queryDrop = "DROP PROCEDURE IF EXISTS SHOW_SUPPLIERS";

    String createProcedure =
        "create procedure SHOW_SUPPLIERS() " +
          "begin " +
            "select SUPPLIERS.SUP_NAME, COFFEES.COF_NAME " +
              "from SUPPLIERS, COFFEES " +
              "where SUPPLIERS.SUP_ID = COFFEES.SUP_ID " +
              "order by SUP_NAME; " +
          "end";

    try (Statement stmtDrop = con.createStatement()) {
      System.out.println("Calling DROP PROCEDURE");
      stmtDrop.execute(queryDrop);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    } 

    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createProcedure);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

请注意，在这个方法中分隔符没有被改变。

存储过程 `SHOW_SUPPLIERS` 生成一个结果集，尽管方法 `createProcedureShowSuppliers` 的返回类型是 `void`，并且该方法不包含任何参数。当使用方法 `CallableStatement.executeQuery` 调用存储过程 `SHOW_SUPPLIERS` 时，会返回一个结果集：

```java
CallableStatement cs = null;
cs = this.con.prepareCall("{call SHOW_SUPPLIERS}");
ResultSet rs = cs.executeQuery();

```

下面从方法 `StoredProcedureMySQLSample.createProcedureGetSupplierOfCoffee` 中提取的内容包含了创建名为 `GET_SUPPLIER_OF_COFFEE` 的存储过程的 SQL 查询：

```java
  public void createProcedureGetSupplierOfCoffee() throws SQLException {

    String queryDrop = "DROP PROCEDURE IF EXISTS GET_SUPPLIER_OF_COFFEE";

    String createProcedure =
        "create procedure GET_SUPPLIER_OF_COFFEE(IN coffeeName varchar(32), OUT supplierName varchar(40)) " +
          "begin " +
            "select SUPPLIERS.SUP_NAME into supplierName " +
              "from SUPPLIERS, COFFEES " +
              "where SUPPLIERS.SUP_ID = COFFEES.SUP_ID " +
              "and coffeeName = COFFEES.COF_NAME; " +
            "select supplierName; " +
          "end";

    try (Statement stmtDrop = con.createStatement()) {
      System.out.println("Calling DROP PROCEDURE");
      stmtDrop.execute(queryDrop);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }

    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createProcedure);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

这个存储过程有两个形式参数，`coffeeName` 和 `supplierName`。参数说明符 `IN` 和 `OUT` 被称为参数模式。它们定义了形式参数的作用。更多信息请参见 参数模式。形式参数在 SQL 查询中定义，而不是在方法 `createProcedureGetSupplierOfCoffee` 中。为了给 `OUT` 参数 `supplierName` 赋值，这个存储过程使用了一个 `SELECT` 语句。

下面从方法 `StoredProcedureMySQLSample.createProcedureRaisePrice` 中提取的内容包含了创建名为 `RAISE_PRICE` 的存储过程的 SQL 查询：

```java
  public void createProcedureRaisePrice() throws SQLException {

    String queryDrop = "DROP PROCEDURE IF EXISTS RAISE_PRICE";

    String createProcedure =
        "create procedure RAISE_PRICE(IN coffeeName varchar(32), IN maximumPercentage float, INOUT newPrice numeric(10,2)) " +
          "begin " +
            "main: BEGIN " +
              "declare maximumNewPrice numeric(10,2); " +
              "declare oldPrice numeric(10,2); " +
              "select COFFEES.PRICE into oldPrice " +
                "from COFFEES " +
                "where COFFEES.COF_NAME = coffeeName; " +
              "set maximumNewPrice = oldPrice * (1 + maximumPercentage); " +
              "if (newPrice > maximumNewPrice) " +
                "then set newPrice = maximumNewPrice; " +
              "end if; " +
              "if (newPrice <= oldPrice) " +
                "then set newPrice = oldPrice;" +
                "leave main; " +
              "end if; " +
              "update COFFEES " +
                "set COFFEES.PRICE = newPrice " +
                "where COFFEES.COF_NAME = coffeeName; " +
              "select newPrice; " +
            "END main; " +
          "end";

    try (Statement stmtDrop = con.createStatement()) {
      System.out.println("Calling DROP PROCEDURE");
      stmtDrop.execute(queryDrop);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }

    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createProcedure);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

存储过程使用 `SET` 和 `SELECT` 语句给 `INOUT` 参数 `newPrice` 赋值。为了退出存储过程，存储过程首先将语句封装在一个标记为 `main` 的 `BEGIN ... END` 块中。为了退出过程，方法使用语句 `leave main`。

### 在 MySQL 中调用存储过程

在 MySQL 中调用存储过程与在 Java DB 中调用它们相同。

下面是从方法`StoredProcedureMySQLSample.runStoredProcedures`中调用存储过程`SHOW_SUPPLIERS`并打印生成的结果集：

```java
      cs = this.con.prepareCall("{call SHOW_SUPPLIERS}");
      ResultSet rs = cs.executeQuery();
      while (rs.next()) {
        String supplier = rs.getString("SUP_NAME");
        String coffee = rs.getString("COF_NAME");
        System.out.println(supplier + ": " + coffee);
      }

```

**注意**：与`Statement`对象一样，要调用存储过程，可以根据过程返回的`ResultSet`对象数量调用`execute`、`executeQuery`或`executeUpdate`。但是，如果不确定过程返回多少个`ResultSet`对象，请调用`execute`。

下面是从方法`StoredProcedureMySQLSample.runStoredProcedures`中调用存储过程`GET_SUPPLIER_OF_COFFEE`的摘录：

```java
      cs = this.con.prepareCall("{call GET_SUPPLIER_OF_COFFEE(?, ?)}");
      cs.setString(1, coffeeNameArg);
      cs.registerOutParameter(2, Types.VARCHAR);
      cs.executeQuery();
      String supplierName = cs.getString(2);

```

接口`CallableStatement`扩展了`PreparedStatement`。它用于调用存储过程。像使用`PreparedStatement`对象一样，通过调用适当的 setter 方法为`IN`参数（例如本例中的`coffeeName`）指定值。但是，如果存储过程包含`OUT`参数，必须使用`registerOutParameter`方法注册它。

下面是从方法`StoredProcedureMySQLSample.runStoredProcedures`中调用存储过程`RAISE_PRICE`的摘录：

```java
      cs = this.con.prepareCall("{call RAISE_PRICE(?,?,?)}");
      cs.setString(1, coffeeNameArg);
      cs.setFloat(2, maximumPercentageArg);
      cs.registerOutParameter(3, Types.NUMERIC);
      cs.setFloat(3, newPriceArg);
      cs.execute();

```

因为参数`newPrice`（过程`RAISE_PRICE`中的第三个参数）具有参数模式`INOUT`，您必须通过调用适当的 setter 方法指定其值，并使用`registerOutParameter`方法注册它。
