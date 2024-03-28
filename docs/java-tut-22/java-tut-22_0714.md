# 入门指南

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/gettingstarted.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/gettingstarted.html)

本教程附带的示例代码创建了一个数据库，供一家名为 The Coffee Break 的小咖啡馆的业主使用，该咖啡馆以磅为单位出售咖啡豆，以杯为单位出售冲泡咖啡。

以下步骤配置了一个 JDBC 开发环境，您可以在其中编译和运行教程示例：

1.  在您的计算机上安装最新版本的 Java SE SDK

1.  如果需要，安装您的数据库管理系统（DBMS）

1.  从您的数据库供应商处安装一个 JDBC 驱动程序

1.  安装 Apache Ant

1.  安装 Apache Xalan

1.  下载示例代码

1.  修改`build.xml`文件

1.  修改教程属性文件

1.  编译和打包示例

1.  创建数据库、表和填充表

1.  运行示例

## 在您的计算机上安装最新版本的 Java SE SDK

在您的计算机上安装最新版本的 Java SE SDK。

确保 Java SE SDK `bin`目录的完整路径在您的`PATH`环境变量中，以便您可以从任何目录运行 Java 编译器和 Java 应用程序启动器。

## 如果需要，请安装您的数据库管理系统（DBMS）

本教程已在以下数据库管理系统上进行测试：

+   [Java DB](https://www.oracle.com/java/technologies/javadb.html)

    **注意**：Java DB 不再包含在最新版本的 JDK 中。Java DB 是 Apache Derby 的一个重新命名。如果您想使用 Java DB，请从[Apache DB 项目](https://db.apache.org/derby/)下载最新版本。

+   [MySQL](https://www.mysql.com/)

请注意，如果您使用其他 DBMS，可能需要修改教程示例的代码。

## 从您的数据库供应商处安装一个 JDBC 驱动程序

如果您使用 Java DB，则已经带有一个 JDBC 驱动程序。如果您使用 MySQL，请安装最新版本的 MySQL JDBC 驱动程序[Connector/J](https://dev.mysql.com/downloads/connector/j/)。

联系您的数据库供应商，获取适用于您的 DBMS 的 JDBC 驱动程序。

JDBC 驱动程序有许多可能的实现。这些实现按以下方式分类：

+   **类型 1**：将 JDBC API 实现为另一个数据访问 API 的映射的驱动程序，例如 ODBC（开放数据库连接）。这种类型的驱动程序通常依赖于本地库，这限制了它们的可移植性。JDBC-ODBC 桥就是类型 1 驱动程序的一个例子。

    **注意**：JDBC-ODBC 桥应被视为一个过渡解决方案。Oracle 不支持它。只有在您的数据库管理系统没有提供仅支持 Java 的 JDBC 驱动程序时才考虑使用它。

+   **Type 2**: 部分用 Java 编程语言编写，部分用本机代码编写的驱动程序。这些驱动程序使用特定于它们连接的数据源的本机客户端库。由于本机代码的存在，它们的可移植性受到限制。Oracle 的 OCI（Oracle Call Interface）客户端驱动程序是 Type 2 驱动程序的一个示例。

+   **Type 3**: 使用纯 Java 客户端并使用数据库独立协议与中间件服务器通信的驱动程序。然后中间件服务器将客户端的请求传达给数据源。

+   **Type 4**: 纯 Java 驱动程序，实现特定数据源的网络协议。客户端直接连接到数据源。

检查你的数据库管理系统中带有哪些驱动程序类型。Java DB 带有两种 Type 4 驱动程序，一个是嵌入式驱动程序，另一个是网络客户端驱动程序。MySQL Connector/J 是一种 Type 4 驱动程序。

安装 JDBC 驱动程序通常包括将驱动程序复制到计算机上，然后将其位置添加到类路径中。此外，除了 Type 4 驱动程序之外的许多 JDBC 驱动程序需要你安装客户端 API。通常不需要其他特殊配置。

## 安装 Apache Ant

这些步骤使用 Apache Ant，一个基于 Java 的工具，来构建、编译和运行 JDBC 教程示例。前往以下链接下载 Apache Ant：

`[`ant.apache.org/`](https://ant.apache.org/)`

确保 Apache Ant 可执行文件在你的`PATH`环境变量中，这样你就可以在任何目录中运行它。

## 安装 Apache Xalan

示例`RSSFeedsTable.java`，在使用 SQLXML 对象中有描述，如果你的数据库管理系统是 Java DB，则需要 Apache Xalan。该示例使用 Apache Xalan-Java。前往以下链接下载：

`[`xml.apache.org/xalan-j/`](https://xml.apache.org/xalan-j/)`

## 下载示例代码

示例代码`JDBCTutorial.zip`包括以下文件：

+   `properties`

    +   `javadb-build-properties.xml`

    +   `javadb-sample-properties.xml`

    +   `mysql-build-properties.xml`

    +   `mysql-sample-properties.xml`

+   `sql`

    +   `javadb`

        +   `create-procedures.sql`

        +   `create-tables.sql`

        +   `drop-tables.sql`

        +   `populate-tables.sql`

    +   `mysql`

        +   `create-procedures.sql`

        +   `create-tables.sql`

        +   `drop-tables.sql`

        +   `populate-tables.sql`

+   `src/com/oracle/tutorial/jdbc`

    +   `CachedRowSetSample.java`

    +   `CityFilter.java`

    +   `ClobSample.java`

    +   `CoffeesFrame.java`

    +   `CoffeesTable.java`

    +   `CoffeesTableModel.java`

    +   `DatalinkSample.java`

    +   `ExampleRowSetListener.java`

    +   `FilteredRowSetSample.java`

    +   `JdbcRowSetSample.java`

    +   `JDBCTutorialUtilities.java`

    +   `JoinSample.java`

    +   `ProductInformationTable.java`

    +   `RSSFeedsTable.java`

    +   `StateFilter.java`

    +   `StoredProcedureJavaDBSample.java`

    +   `StoredProcedureMySQLSample.java`

    +   `SuppliersTable.java`

    +   `WebRowSetSample.java`

+   `txt`

    +   `colombian-description.txt`

+   `xml`

    +   `rss-coffee-industry-news.xml`

    +   `rss-the-coffee-break-blog.xml`

+   `build.xml`

创建一个目录来包含示例的所有文件。这些步骤将此目录称为 `*<JDBC 教程目录>*`。将 `JDBCTutorial.zip` 的内容解压缩到 `*<JDBC 教程目录>*` 中。

## 修改 build.xml 文件

`build.xml` 文件是 Apache Ant 用于编译和执行 JDBC 示例的构建文件。文件 `properties/javadb-build-properties.xml` 和 `properties/mysql-build-properties.xml` 包含 Java DB 和 MySQL 需要的额外 Apache Ant 属性。文件 `properties/javadb-sample-properties.xml` 和 `properties/mysql-sample-properties.xml` 包含示例所需的属性。

修改这些 XML 文件如下：

### 修改 build.xml

在 `build.xml` 文件中，修改属性 `ANTPROPERTIES`，指向 `properties/javadb-build-properties.xml` 或 `properties/mysql-build-properties.xml`，取决于您的 DBMS。例如，如果您正在使用 Java DB，则您的 `build.xml` 文件将包含以下内容：

```java
<property
  name="ANTPROPERTIES"
  value="properties/javadb-build-properties.xml"/>

  <import file="${ANTPROPERTIES}"/>

```

同样，如果您正在使用 MySQL，您的 `build.xml` 文件将包含以下内容：

```java
<property
  name="ANTPROPERTIES"
  value="properties/mysql-build-properties.xml"/>

  <import file="${ANTPROPERTIES}"/>

```

### 修改特定于数据库的属性文件

在 `properties/javadb-build-properties.xml` 或 `properties/mysql-build-properties.xml` 文件（取决于您的 DBMS），根据以下表格中的描述修改以下属性：

| 属性 | 描述 |
| --- | --- |
| `JAVAC` | 您的 Java 编译器 `javac` 的完整路径名。 |
| `JAVA` | 您的 Java 运行时可执行文件 `java` 的完整路径名。 |
| `PROPERTIESFILE` | 属性文件的名称，可以是 `properties/javadb-sample-properties.xml` 或 `properties/mysql-sample-properties.xml`。 |
| `MYSQLDRIVER` | 你的 MySQL 驱动程序的完整路径名。对于 Connector/J，通常是 `*<Connector/J 安装目录>*/mysql-connector-java-*版本号*.jar`。 |
| `JAVADBDRIVER` | 您的 Java DB 驱动程序的完整路径名。通常是 `*<Java DB 安装目录>*/lib/derby.jar`。 |
| `XALANDIRECTORY` | 包含 Apache Xalan 的目录的完整路径名。 |
| `CLASSPATH` | JDBC 教程使用的类路径。*您无需更改此值*。 |
| `XALAN` | 文件 `xalan.jar` 的完整路径名。 |
| `DB.VENDOR` | 一个值，可以是 `derby` 或 `mysql`，取决于您是使用 Java DB 还是 MySQL。教程使用此值来构建连接到 DBMS 和识别特定于 DBMS 的代码和 SQL 语句所需的 URL。 |
| `DB.DRIVER` | JDBC 驱动程序的完全限定类名。对于 Java DB，这是 `org.apache.derby.jdbc.EmbeddedDriver`。对于 MySQL，这是 `com.mysql.cj.jdbc.Driver`。 |
| `DB.HOST` | 托管您的 DBMS 的计算机的主机名。 |
| `DB.PORT` | 托管您的 DBMS 的计算机的端口号。 |
| `DB.SID` | 教程创建和使用的数据库名称。 |
| `DB.URL.NEWDATABASE` | 创建新数据库时用于连接到您的 DBMS 的连接 URL。*您无需更改此值*。 |
| `DB.URL` | 用于连接到您的 DBMS 的连接 URL。*您无需更改此值*。 |
| `DB.USER` | 具有在 DBMS 中创建数据库权限的用户的名称。 |
| `DB.PASSWORD` | 指定在`DB.USER`中的用户的密码。 |
| `DB.DELIMITER` | 用于分隔 SQL 语句的字符。*不要更改此值*。它应该是分号字符（`;`）。 |

## 修改教程属性文件

教程示例使用`properties/javadb-sample-properties.xml`文件或`properties/mysql-sample-properties.xml`文件中的值（取决于您的 DBMS）来连接到 DBMS 并初始化数据库和表，如下表所述：

| 属性 | 描述 |
| --- | --- |
| `dbms` | 取值为`derby`或`mysql`，取决于您是使用 Java DB 还是 MySQL。本教程使用此值来构建连接到 DBMS 所需的 URL，并识别 DBMS 特定的代码和 SQL 语句。 |
| `jar_file` | 包含本教程所有类文件的 JAR 文件的完整路径名。 |
| `driver` | JDBC 驱动程序的完全限定类名。对于 Java DB，这是`org.apache.derby.jdbc.EmbeddedDriver`。对于 MySQL，这是`com.mysql.cj.jdbc.Driver`。 |
| `database_name` | 教程创建和使用的数据库名称。 |
| `user_name` | 具有在 DBMS 中创建数据库权限的用户的名称。 |
| `password` | 指定在`user_name`中的用户的密码。 |
| `server_name` | 托管您的 DBMS 的计算机的主机名。 |
| `port_number` | 托管您的 DBMS 的计算机的端口号。 |

**注意**：为了简化演示 JDBC API，JDBC 教程示例代码不执行部署系统通常使用的密码管理技术。在生产环境中，您可以遵循 Oracle 数据库密码管理指南并禁用任何示例帐户。请参阅*Oracle 数据库安全指南*中的[应用程序设计中的密码保护](https://docs.oracle.com/en/database/oracle/oracle-database/20/dbseg/managing-security-for-application-developers.html#GUID-DE90CA6F-F37B-4337-B331-4FA0F7C7599A)部分，了解密码管理指南和其他安全建议。

## 编译并打包示例

在命令提示符下，将当前目录更改为`*<JDBC 教程目录>*`。从该目录运行以下命令编译示例并将其打包到一个 jar 文件中：

```java
ant jar

```

## 创建数据库、表格，并填充表格

如果您使用的是 MySQL，则运行以下命令来创建数据库：

```java
ant create-mysql-database

```

**注意**：`build.xml`文件中不存在用于为 Java DB 创建数据库的相应 Ant 目标。用于建立数据库连接的 Java DB 数据库 URL 包括创建数据库的选项（如果尚不存在）。有关更多信息，请参阅建立连接。

如果您正在使用 Java DB 或 MySQL，则可以从同一目录运行以下命令来删除现有的示例数据库表，重新创建表并填充它们。对于 Java DB，此命令还会在数据库不存在时创建数据库：

```java
ant setup

```

**注意**：在运行示例中的 Java 类之前，您应该每次运行`ant setup`命令。这些示例中的许多示例都期望示例数据库表的内容中有特定的数据。

## 运行示例

`build.xml`文件中的每个目标对应于 JDBC 示例中的一个 Java 类或 SQL 脚本。以下表列出了`build.xml`文件中的目标，每个目标执行的类或脚本，以及每个目标需要的其他类或文件：

| Ant 目标 | 类或 SQL 脚本 | 其他必需类或文件 |
| --- | --- | --- |
| `javadb-create-procedure` | `javadb/create-procedures.sql`；查看`build.xml`文件以查看运行的其他 SQL 语句 | 无其他必需文件 |
| `mysql-create-procedure` | `mysql/create-procedures.sql` | 无其他必需文件 |
| `run` | `JDBCTutorialUtilities` | 无其他必需类 |
| `runct` | `CoffeesTable` | `JDBCTutorialUtilities` |
| `runst` | `SuppliersTable` | `JDBCTutorialUtilities` |
| `runjrs` | `JdbcRowSetSample` | `JDBCTutorialUtilities` |
| `runcrs` | `CachedRowSetSample`，`ExampleRowSetListener` | `JDBCTutorialUtilities` |
| `runjoin` | `JoinSample` | `JDBCTutorialUtilities` |
| `runfrs` | `FilteredRowSetSample` | `JDBCTutorialUtilities`，`CityFilter`，`StateFilter` |
| `runwrs` | `WebRowSetSample` | `JDBCTutorialUtilities` |
| `runclob` | `ClobSample` | `JDBCTutorialUtilities`，`txt/colombian-description.txt` |
| `runrss` | `RSSFeedsTable` | `JDBCTutorialUtilities`，`xml`目录中包含的 XML 文件 |
| `rundl` | `DatalinkSample` | `JDBCTutorialUtilities` |
| `runspjavadb` | `StoredProcedureJavaDBSample` | `JDBCTutorialUtilities`，`SuppliersTable`，`CoffeesTable` |
| `runspmysql` | `StoredProcedureMySQLSample` | `JDBCTutorialUtilities`，`SuppliersTable`，`CoffeesTable` |
| `runframe` | `CoffeesFrame` | `JDBCTutorialUtilities`，`CoffeesTableModel` |

例如，要运行`CoffeesTable`类，请将当前目录更改为`*<JDBC 教程目录>*`，然后从该目录运行以下命令：

```java
ant runct

```
