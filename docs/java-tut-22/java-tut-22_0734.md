# 使用结构化对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/sqlstructured.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqlstructured.html)

**注意**：MySQL 和 Java DB 目前不支持用户定义类型。因此，没有 JDBC 教程示例可用来演示本节描述的功能。

下列主题包括：

+   结构化类型概述

+   在结构化类型中使用 DISTINCT 类型

+   使用结构化类型的引用

+   创建 SQL REF 对象的示例代码

+   将用户定义类型用作列值

+   将用户定义类型插入表中

## 结构化类型概述

SQL 结构化类型和`DISTINCT`类型是用户可以在 SQL 中定义的两种数据类型。它们通常被称为 UDT（用户定义类型），您可以使用 SQL 的`CREATE` `TYPE`语句来创建它们。

回到 The Coffee Break 的例子，假设所有者的成功超出了所有预期，并且一直在扩张新分店。所有者决定向数据库添加一个`STORES`表，其中包含有关每个机构的信息。`STORES`将有四列：

+   `STORE_NO`代表每个店铺的识别号

+   `LOCATION`代表其地址

+   `COF_TYPES`代表其销售的咖啡

+   `MGR`代表店长的姓名

所有者将`LOCATION`列设为 SQL 结构化类型，`COF_TYPES`列设为 SQL `ARRAY`，`MGR`列设为`REF(MANAGER)`，其中`MANAGER`是 SQL 结构化类型。

所有者首先必须为地址和经理定义新的结构化类型。SQL 结构化类型类似于 Java 编程语言中的结构化类型，它具有称为*属性*的成员，可以是任何数据类型。所有者编写以下 SQL 语句来创建新数据类型`ADDRESS`：

```java
CREATE TYPE ADDRESS
(
    NUM INTEGER,
    STREET VARCHAR(40),
    CITY VARCHAR(40),
    STATE CHAR(2),
    ZIP CHAR(5)
);

```

在此语句中，新类型`ADDRESS`有五个属性，类似于 Java 类中的字段。属性`NUM`是一个`INTEGER`，属性`STREET`是一个`VARCHAR(40)`，属性`CITY`是一个`VARCHAR(40)`，属性`STATE`是一个`CHAR(2)`，属性`ZIP`是一个`CHAR(5)`。

下面的摘录中，`con`是一个有效的`Connection`对象，将`ADDRESS`的定义发送到数据库：

```java
String createAddress =
    "CREATE TYPE ADDRESS " +
    "(NUM INTEGER, STREET VARCHAR(40), " +
    "CITY VARCHAR(40), STATE CHAR(2), ZIP CHAR(5))";
Statement stmt = con.createStatement();
stmt.executeUpdate(createAddress);

```

现在`ADDRESS`结构化类型已在数据库中注册为数据类型，所有者可以将其用作表列或结构化类型属性的数据类型。

## 在结构化类型中使用 DISTINCT 类型

The Coffee Break 的所有者计划在新的结构化类型`MANAGER`中包含的属性之一是经理的电话号码。因为所有者总是将电话号码列为一个 10 位数（以确保包括区号），并且永远不会将其作为数字进行操作，所以所有者决定定义一个名为`PHONE_NO`的新类型，该类型由 10 个字符组成。这种数据类型的 SQL 定义，可以被视为只有一个属性的结构化类型，如下所示：

```java
CREATE TYPE PHONE_NO AS CHAR(10);

```

或者，如前面提到的，对于某些驱动程序，定义可能如下所示：

```java
CREATE DISTINCT TYPE PHONE_NO AS CHAR(10);

```

`DISTINCT`类型始终基于另一个数据类型，该数据类型必须是预定义类型。换句话说，`DISTINCT`类型不能基于用户定义类型（UDT）。要检索或设置`DISTINCT`类型的值，请使用基础类型（其基础类型）的适当方法。例如，要检索基于`CHAR`类型的`PHONE_NO`实例，您将使用`getString`方法，因为这是检索`CHAR`的方法。

假设`ResultSet`对象`*rs*`的当前行的第四列中有一个类型为`PHONE_NO`的值，以下代码行检索它：

```java
String phoneNumber = rs.getString(4);

```

类似地，以下代码行设置了一个输入参数，该参数具有类型`PHONE_NO`，用于发送到数据库的预备语句：

```java
pstmt.setString(1, phoneNumber);

```

在前面的代码片段中添加，`PHONE_NO`的定义将通过以下代码行发送到数据库：

```java
stmt.executeUpdate(
    "CREATE TYPE PHONE_NO AS CHAR(10)");

```

在向数据库注册类型`PHONE_NO`之后，所有者可以将其用作表中的列类型或作为结构化类型中属性的数据类型。以下 SQL 语句中`MANAGER`的定义使用`PHONE_NO`作为属性`PHONE`的数据类型：

```java
CREATE TYPE MANAGER
(
    MGR_ID INTEGER,
    LAST_NAME VARCHAR(40),
    FIRST_NAME VARCHAR(40),
    PHONE PHONE_NO
);

```

重用之前定义的`*stmt*`，以下代码片段将结构化类型`MANAGER`的定义发送到数据库：

```java
  String createManager =
    "CREATE TYPE MANAGER " +
    "(MGR_ID INTEGER, LAST_NAME " +
    "VARCHAR(40), " +
    "FIRST_NAME VARCHAR(40), " +
    "PHONE PHONE_NO)";
  stmt.executeUpdate(createManager);

```

## 使用结构化类型的引用

The Coffee Break 的所有者创建了三种新的数据类型，用作数据库中的列类型或属性类型：结构化类型`LOCATION`和`MANAGER`，以及`DISTINCT`类型`PHONE_NO`。企业家将`PHONE_NO`用作新类型`MANAGER`中属性`PHONE`的类型，并将`ADDRESS`用作表`STORES`中列`LOCATION`的数据类型。`MANAGER`类型可以用作列`MGR`的类型，但企业家更喜欢使用类型`REF(MANAGER)`，因为企业家经常让一个人管理两到三家店铺。将`REF(MANAGER)`用作列类型可以避免在一个人管理多家店铺时重复所有`MANAGER`的数据。

已经创建了结构化类型 `MANAGER`，所有者现在可以创建一个包含可以被引用的 `MANAGER` 实例的表。对 `MANAGER` 实例的引用将具有类型 `REF(MANAGER)`。SQL `REF` 只不过是指向结构化类型的逻辑指针，因此 `REF(MANAGER)` 实例充当对 `MANAGER` 实例的逻辑指针。

因为 SQL `REF` 值需要永久与其引用的结构化类型实例关联在一起，所以它存储在一个特殊的表中，与其关联的实例一起。程序员不直接创建 `REF` 类型，而是创建将存储特定结构化类型实例的表，这些实例可以被引用。每个要被引用的结构化类型都将有自己的表。当你将结构化类型的实例插入表中时，数据库会自动创建一个 `REF` 实例。例如，为了包含可以被引用的 `MANAGER` 实例，所有者使用 SQL 创建了以下特殊表：

```java
  CREATE TABLE MANAGERS OF MANAGER
  (OID REF(MANAGER)
  VALUES ARE SYSTEM GENERATED);

```

这个语句创建了一个带有特殊列 `OID` 的表，该列存储 `REF(MANAGER)` 类型的值。每次将 `MANAGER` 实例插入表中时，数据库都会生成一个 `REF(MANAGER)` 实例并将其存储在列 `OID` 中。隐式地，另外一列存储已插入表中的 `MANAGER` 的每个属性。例如，以下代码片段展示了企业家如何创建了三个 `MANAGER` 结构化类型的实例来代表三个经理：

```java
  INSERT INTO MANAGERS (
    MGR_ID, LAST_NAME,
    FIRST_NAME, PHONE) VALUES
  (
    000001,
    'MONTOYA',
    'ALFREDO',
    '8317225600'
  );

  INSERT INTO MANAGERS (
    MGR_ID, LAST_NAME,
    FIRST_NAME, PHONE) VALUES
  (
    000002,
    'HASKINS',
    'MARGARET',
    '4084355600'
  );

  INSERT INTO MANAGERS (
    MGR_ID, LAST_NAME,
    FIRST_NAME, PHONE) VALUES
  (
    000003,
    'CHEN',
    'HELEN',
    '4153785600'
   );

```

表 `MANAGERS` 现在将有三行，每个已插入的经理一行。列 `OID` 将包含三个 `REF(MANAGER)` 类型的唯一对象标识符，每个 `MANAGER` 实例一个。这些对象标识符是由数据库自动生成的，并将永久存储在表 `MANAGERS` 中。隐式地，另外一列存储 `MANAGER` 的每个属性。例如，在表 `MANAGERS` 中，一行包含一个引用 Alfredo Montoya 的 `REF(MANAGER)`，另一行包含一个引用 Margaret Haskins 的 `REF(MANAGER)`，第三行包含一个引用 Helen Chen 的 `REF(MANAGER)`。

要访问 `REF(MANAGER)` 实例，您可以从其表中选择。例如，所有者使用以下代码片段检索了对 ID 号为 000001 的 Alfredo Montoya 的引用：

```java
  String selectMgr =
    "SELECT OID FROM MANAGERS " +
    "WHERE MGR_ID = 000001";
  ResultSet rs = stmt.executeQuery(selectMgr);
  rs.next();
  Ref manager = rs.getRef("OID");

```

现在变量 `*manager*` 可以被用作引用 Alfredo Montoya 的列值。

## 创建 SQL REF 对象的示例代码

以下代码示例创建了表`MANAGERS`，这是结构化类型`MANAGER`的实例表，可以引用，并将三个`MANAGER`实例插入表中。此表中的列`OID`将存储`REF(MANAGER)`的实例。执行此代码后，`MANAGERS`表将为插入的三个`MANAGER`对象的每个对象插入一行，并且`OID`列中的值将是标识存储在该行中的`MANAGER`实例的`REF(MANAGER)`类型。

```java
package com.oracle.tutorial.jdbc;

import java.sql.*;

public class CreateRef {

    public static void main(String args[]) {

        JDBCTutorialUtilities myJDBCTutorialUtilities;
        Connection myConnection = null;

        if (args[0] == null) {
            System.err.println("Properties file not specified " +
                               "at command line");
            return;
        } else {
            try {
                myJDBCTutorialUtilities = new JDBCTutorialUtilities(args[0]);
            } catch (Exception e) {
                System.err.println("Problem reading properties " +
                                   "file " + args[0]);
                e.printStackTrace();
                return;
            }
        }

        Connection con = null;
        Statement stmt = null;

        try {
            String createManagers =
                "CREATE TABLE " +
                "MANAGERS OF MANAGER " +
                "(OID REF(MANAGER) " +
                "VALUES ARE SYSTEM " +
                "GENERATED)";

            String insertManager1 =
                "INSERT INTO MANAGERS " +
                "(MGR_ID, LAST_NAME, " +
                "FIRST_NAME, PHONE) " +
                "VALUES " +
                "(000001, 'MONTOYA', " +
                "'ALFREDO', " +
                "'8317225600')";

            String insertManager2 =
                "INSERT INTO MANAGERS " +
                "(MGR_ID, LAST_NAME, " +
                "FIRST_NAME, PHONE) " +
                "VALUES " +
                "(000002, 'HASKINS', " +
                "'MARGARET', " +
                "'4084355600')";

            String insertManager3 =
                "INSERT INTO MANAGERS " +
                "(MGR_ID, LAST_NAME, " +
                "FIRST_NAME, PHONE) " +
                "VALUES " +
                "(000003, 'CHEN', 'HELEN', " +
                "'4153785600')";

            con = myJDBCTutorialUtilities.getConnection();
            con.setAutoCommit(false);

            stmt = con.createStatement();
            stmt.executeUpdate(createManagers);

            stmt.addBatch(insertManager1);
            stmt.addBatch(insertManager2);
            stmt.addBatch(insertManager3);
            int [] updateCounts = stmt.executeBatch();

            con.commit();

            System.out.println("Update count for:  ");
            for (int i = 0; i < updateCounts.length; i++) {
                System.out.print("    command " + (i + 1) + " = ");
                System.out.println(updateCounts[i]);
            }
        } catch(BatchUpdateException b) {
            System.err.println("-----BatchUpdateException-----");
            System.err.println("Message:  " + b.getMessage());
            System.err.println("SQLState:  " + b.getSQLState());
            System.err.println("Vendor:  " + b.getErrorCode());
            System.err.print("Update counts for " + "successful commands:  ");
            int [] rowsUpdated = b.getUpdateCounts();
            for (int i = 0; i < rowsUpdated.length; i++) {
                System.err.print(rowsUpdated[i] + "   ");
            }
            System.err.println("");
        } catch(SQLException ex) {
            System.err.println("------SQLException------");
            System.err.println("Error message:  " + ex.getMessage());
            System.err.println("SQLState:  " + ex.getSQLState());
            System.err.println("Vendor:  " + ex.getErrorCode());
        } finally {
            if (stmt != null) { stmt.close(); }
              JDBCTutorialUtilities.closeConnection(con);
        }
    }
}

```

## 使用用户定义类型作为列值

我们的企业家现在拥有创建表`STORES`所需的 UDT。结构化类型`ADDRESS`是列`LOCATION`的类型，类型`REF(MANAGER)`是列`MGR`的类型。

UDT `COF_TYPES`基于 SQL 数据类型`ARRAY`，是列`COF_TYPES`的类型。以下代码行创建了类型`COF_ARRAY`作为具有 10 个元素的`ARRAY`值。`COF_ARRAY`的基本类型是`VARCHAR(40)`。

```java
  CREATE TYPE COF_ARRAY AS ARRAY(10) OF VARCHAR(40);

```

定义了新数据类型后，以下 SQL 语句创建了表`STORES`：

```java
  CREATE TABLE STORES
  (
    STORE_NO INTEGER,
    LOCATION ADDRESS,
    COF_TYPES COF_ARRAY,
    MGR REF(MANAGER)
  );

```

## 将用户定义类型插入表中

以下代码片段向`STORES`表中插入一行，按顺序提供了列`STORE_NO`，`LOCATION`，`COF_TYPES`和`MGR`的值：

```java
  INSERT INTO STORES VALUES
  (
    100001,
    ADDRESS(888, 'Main_Street',
      'Rancho_Alegre',
      'CA', '94049'),
    COF_ARRAY('Colombian', 'French_Roast',
      'Espresso', 'Colombian_Decaf',
      'French_Roast_Decaf'),
    SELECT OID FROM MANAGERS
      WHERE MGR_ID = 000001
  );

```

以下逐个列出每列及其插入的值。

```java
  STORE_NO: 100001

```

此列类型为`INTEGER`，数字`100001`是`INTEGER`类型，类似于之前在`COFFEES`和`SUPPLIERS`表中插入的条目。

```java
  LOCATION: ADDRESS(888, 'Main_Street',
    'Rancho_Alegre', 'CA', '94049')

```

此列的类型为结构化类型`ADDRESS`，此值是`ADDRESS`实例的构造函数。当我们将`ADDRESS`的定义发送到数据库时，其中一件事是为新类型创建构造函数。括号中的逗号分隔值是`ADDRESS`类型属性的初始化值，它们必须按照`ADDRESS`类型定义中属性列出的顺序出现。`888`是属性`NUM`的值，是`INTEGER`值。`"Main_Street"`是`STREET`的值，`"Rancho_Alegre"`是`CITY`的值，这两个属性都是`VARCHAR(40)`类型。属性`STATE`的值为`"CA"`，是`CHAR(2)`类型，属性`ZIP`的值为`"94049"`，是`CHAR(5)`类型。

```java
  COF_TYPES: COF_ARRAY(
    'Colombian',
    'French_Roast',
    'Espresso',
    'Colombian_Decaf',
    'French_Roast_Decaf'),

```

列`COF_TYPES`的类型为`COF_ARRAY`，基本类型为`VARCHAR(40)`，括号中的逗号分隔值是数组元素为`String`对象。所有者定义了类型`COF_ARRAY`最多有 10 个元素。此数组有 5 个元素，因为企业家仅提供了 5 个`String`对象。

```java
  MGR: SELECT OID FROM MANAGERS
    WHERE MGR_ID = 000001

```

列`MGR`的类型是`REF(MANAGER)`，这意味着该列中的值必须是指向结构化类型`MANAGER`的引用。所有`MANAGER`的实例都存储在表`MANAGERS`中。所有`REF(MANAGER)`的实例也存储在该表中，存储在列`OID`中。此表行中描述的商店的经理是 Alfredo Montoya，他的信息存储在具有属性`MGR_ID`为`100001`的`MANAGER`实例中。要获取与 Alfredo Montoya 的`MANAGER`对象关联的`REF(MANAGER)`实例，请选择表`MANAGERS`中`MGR_ID`为`100001`的行中的列`OID`。将存储在`STORES`表的`MGR`列中的值（`REF(MANAGER)`值）是 DBMS 生成的用于唯一标识此`MANAGER`结构化类型实例的值。

将上述 SQL 语句发送到数据库，使用以下代码片段：

```java
  String insertMgr =
    "INSERT INTO STORES VALUES " +
    "(100001, " +
    "ADDRESS(888, 'Main_Street', " +
      "'Rancho_Alegre', 'CA', " +
      "'94049'), " +
    "COF_ARRAY('Colombian', " +
      "'French_Roast', 'Espresso', " +
      "'Colombian_Decaf', " +
      "'French_Roast_Decaf'}, " +
    "SELECT OID FROM MANAGERS " +
    "WHERE MGR_ID = 000001)";

  stmt.executeUpdate(insertMgr);

```

然而，因为你将发送多个`INSERT INTO`语句，将它们一起作为批量更新发送会更有效，就像以下代码示例中所示：

```java
package com.oracle.tutorial.jdbc;

import java.sql.*;

public class InsertStores {
    public static void main(String args[]) {

        JDBCTutorialUtilities myJDBCTutorialUtilities;
        Connection myConnection = null;

        if (args[0] == null) {
            System.err.println(
                "Properties file " +
                "not specified " +
                "at command line");
            return;
        } else {
            try {
                myJDBCTutorialUtilities = new
                    JDBCTutorialUtilities(args[0]);
            } catch (Exception e) {
                System.err.println(
                    "Problem reading " +
                    "properties file " +
                    args[0]);
                e.printStackTrace();
                return;
            }
        }

        Connection con = null;
        Statement stmt = null;

        try {
            con = myJDBCTutorialUtilities.getConnection();
            con.setAutoCommit(false);

            stmt = con.createStatement();

            String insertStore1 =
                "INSERT INTO STORES VALUES (" +
                "100001, " +
                "ADDRESS(888, 'Main_Street', " +
                    "'Rancho_Alegre', 'CA', " +
                    "'94049'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " +
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000001))";

            stmt.addBatch(insertStore1);

            String insertStore2 =
                "INSERT INTO STORES VALUES (" +
                "100002, " +
                "ADDRESS(1560, 'Alder', " +
                    "'Ochos_Pinos', " +
                    "'CA', '94049'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " +
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf', " +
                    "'Kona', 'Kona_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000001))";

            stmt.addBatch(insertStore2);

            String insertStore3 =
                "INSERT INTO STORES VALUES (" +
                "100003, " +
                "ADDRESS(4344, " +
                    "'First_Street', " +
                    "'Verona', " +
                    "'CA', '94545'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " +
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf', " +
                    "'Kona', 'Kona_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000002))";

            stmt.addBatch(insertStore3);

            String insertStore4 =
                "INSERT INTO STORES VALUES (" +
                "100004, " +
                "ADDRESS(321, 'Sandy_Way', " +
                    "'La_Playa', " +
                    "'CA', '94544'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " +
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf', " +
                    "'Kona', 'Kona_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000002))";

            stmt.addBatch(insertStore4);

            String insertStore5 =
                "INSERT INTO STORES VALUES (" +
                "100005, " +
                "ADDRESS(1000, 'Clover_Road', " +
                    "'Happyville', " +
                    "'CA', '90566'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " + 
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000003))";

            stmt.addBatch(insertStore5);

            int [] updateCounts = stmt.executeBatch();

            ResultSet rs = stmt.executeQuery(
                "SELECT * FROM STORES");
            System.out.println("Table STORES after insertion:");
            System.out.println("STORE_NO   " + "LOCATION   " +
                "COF_TYPE   " + "MGR");

            while (rs.next()) {
                int storeNo = rs.getInt("STORE_NO");
                Struct location = (Struct)rs.getObject("LOCATION");
                Object[] locAttrs = location.getAttributes();
                Array coffeeTypes = rs.getArray("COF_TYPE");
                String[] cofTypes = (String[])coffeeTypes.getArray();

                Ref managerRef = rs.getRef("MGR");
                PreparedStatement pstmt = con.prepareStatement(
                    "SELECT MANAGER " +
                    "FROM MANAGERS " +
                    "WHERE OID = ?");

                pstmt.setRef(1, managerRef);
                ResultSet rs2 = pstmt.executeQuery();
                rs2.next();
                Struct manager = (Struct)rs2.getObject("MANAGER");
                Object[] manAttrs = manager.getAttributes();

                System.out.print(storeNo + "   ");
                System.out.print(
                    locAttrs[0] + " " +
                    locAttrs[1] + " " +
                    locAttrs[2] + ", " +
                    locAttrs[3] + " " +
                    locAttrs[4] + " ");

                for (int i = 0; i < cofTypes.length; i++)
                    System.out.print( cofTypes[i] + " ");

                System.out.println(
                    manAttrs[1] + ", " +
                    manAttrs[2]);

                rs2.close();
                pstmt.close();
            }

            rs.close();

        } catch(BatchUpdateException b) {
            System.err.println("-----BatchUpdateException-----");
            System.err.println("SQLState:  " + b.getSQLState());
            System.err.println("Message:  " + b.getMessage());
            System.err.println("Vendor:  " + b.getErrorCode());
            System.err.print("Update counts:  ");
            int [] updateCounts = b.getUpdateCounts();

            for (int i = 0; i < updateCounts.length; i++) {
                System.err.print(updateCounts[i] + "   ");
            }
            System.err.println("");

        } catch(SQLException ex) {
            System.err.println("SQLException: " + ex.getMessage());
            System.err.println("SQLState:  " + ex.getSQLState());
            System.err.println("Message:  " + ex.getMessage());
            System.err.println("Vendor:  " + ex.getErrorCode());
        } finally {
            if (stmt != null) { stmt.close(); }
                JDBCTutorialUtilities.closeConnection(con);
            }
        }
    }
}

```
