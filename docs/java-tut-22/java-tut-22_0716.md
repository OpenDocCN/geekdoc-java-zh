# 建立连接

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html)

首先，您需要与要使用的数据源建立连接。数据源可以是 DBMS、传统文件系统或其他具有相应 JDBC 驱动程序的数据源。通常，JDBC 应用程序使用以下两个类之一连接到目标数据源：

+   `DriverManager`：此完全实现的类将应用程序连接到由数据库 URL 指定的数据源。当此类首次尝试建立连接时，它会自动加载类路径中找到的任何 JDBC 4.0 驱动程序。请注意，您的应用程序必须在版本 4.0 之前手动加载任何 JDBC 驱动程序。

+   `DataSource`：此接口优先于`DriverManager`，因为它允许底层数据源的详细信息对您的应用程序透明。设置`DataSource`对象的属性，使其表示特定的数据源。有关更多信息，请参阅使用 DataSource 对象进行连接。有关使用`DataSource`类开发应用程序的更多信息，请参阅最新版本的*[Java EE 教程](https://javaee.github.io/tutorial/toc.html)*。

**注意**：本教程中的示例使用`DriverManager`类而不是`DataSource`类，因为它更容易使用，而且示例不需要`DataSource`类的功能。

本页涵盖以下主题：

+   使用 DriverManager 类

+   指定数据库连接 URL

## 使用 DriverManager 类

使用`DriverManager`类连接到您的 DBMS 涉及调用方法`DriverManager.getConnection`。以下方法，`JDBCTutorialUtilities.getConnection`，建立数据库连接：

```java
public Connection getConnection() throws SQLException {

    Connection conn = null;
    Properties connectionProps = new Properties();
    connectionProps.put("user", this.userName);
    connectionProps.put("password", this.password);

    if (this.dbms.equals("mysql")) {
        conn = DriverManager.getConnection(
                   "jdbc:" + this.dbms + "://" +
                   this.serverName +
                   ":" + this.portNumber + "/",
                   connectionProps);
    } else if (this.dbms.equals("derby")) {
        conn = DriverManager.getConnection(
                   "jdbc:" + this.dbms + ":" +
                   this.dbName +
                   ";create=true",
                   connectionProps);
    }
    System.out.println("Connected to database");
    return conn;
}

```

方法`DriverManager.getConnection`建立数据库连接。此方法需要数据库 URL，具体取决于您的 DBMS。以下是一些数据库 URL 的示例：

1.  MySQL：`jdbc:mysql://localhost:3306/`，其中`localhost`是托管数据库的服务器名称，`3306`是端口号

1.  Java DB：`jdbc:derby:*testdb*;create=true`，其中`*testdb*`是要连接的数据库的名称，`create=true`指示 DBMS 创建数据库。

    **注意**：此 URL 与 Java DB 嵌入式驱动程序建立数据库连接。Java DB 还包括使用不同 URL 的网络客户端驱动程序。

此方法使用`Properties`对象指定访问 DBMS 所需的用户名和密码。

**注意**：

+   通常，在数据库 URL 中，你还会指定要连接的现有数据库的名称。例如，URL `jdbc:mysql://localhost:3306/mysql` 代表了名为 `mysql` 的 MySQL 数据库的数据库 URL。本教程中的示例使用不指定特定数据库的 URL，因为示例会创建一个新数据库。

+   在以前的 JDBC 版本中，要获取连接，你首先需要通过调用方法 `Class.forName` 来初始化你的 JDBC 驱动程序。这些方法需要一个类型为 `java.sql.Driver` 的对象。每个 JDBC 驱动程序都包含一个或多个实现接口 `java.sql.Driver` 的类。Java DB 的驱动程序是 `org.apache.derby.jdbc.EmbeddedDriver` 和 `org.apache.derby.jdbc.ClientDriver`，而 MySQL Connector/J 的驱动程序是 `com.mysql.cj.jdbc.Driver`。查看你的 DBMS 驱动程序的文档以获取实现接口 `java.sql.Driver` 的类名。

    任何在你的类路径中找到的 JDBC 4.0 驱动程序都会自动加载。（但是，在 JDBC 4.0 之前，你必须手动加载任何驱动程序，使用方法 `Class.forName`。）

该方法返回一个 `Connection` 对象，表示与 DBMS 或特定数据库的连接。通过这个对象查询数据库。

## 指定数据库连接 URL

数据库连接 URL 是你的 DBMS JDBC 驱动程序用来连接数据库的字符串。它可以包含诸如在哪里搜索数据库、要连接的数据库名称和配置属性等信息。数据库连接 URL 的确切语法由你的 DBMS 指定。

### Java DB 数据库连接 URL

以下是 Java DB 的数据库连接 URL 语法：

```java
jdbc:derby:[*subsubprotocol*:][*databaseName*][;*attribute*=*value*]*

```

+   `*subsubprotocol*` 指定 Java DB 应在何处搜索数据库，可以是目录、内存、类路径或 JAR 文件。通常会省略。

+   `*databaseName*` 是要连接的数据库的名称。

+   `*attribute*=*value*` 表示一个可选的、以分号分隔的属性列表。这些属性使你能够指示 Java DB 执行各种任务，包括以下内容：

    +   创建连接 URL 中指定的数据库。

    +   加密连接 URL 中指定的数据库。

    +   指定存储日志和跟踪信息的目录。

    +   指定用户名和密码以连接到数据库。

查看[Java DB 技术文档](https://docs.oracle.com/javadb/index_jdk8.html)中的*Java DB 开发人员指南*和*Java DB 参考手册*以获取更多信息。

### MySQL Connector/J 数据库 URL

以下是 MySQL Connector/J 的数据库连接 URL 语法：

```java
jdbc:mysql://[*host*][,*failoverhost*...]
    [:*port*]/[*database*]
    [?*propertyName1*][=*propertyValue1*]
    [&*propertyName2*][=*propertyValue2*]...

```

+   `*host*:*port*` 是托管数据库的计算机的主机名和端口号。如果未指定，默认值为 `*host*` 和 `*port*` 分别为 127.0.0.1 和 3306。

+   `*database*` 是要连接的数据库的名称。如果未指定，将连接到没有默认数据库的连接。

+   `*failover*` 是一个备用数据库的名称（MySQL Connector/J 支持故障转移）。

+   `*propertyName*=*propertyValue*` 表示一个可选的、以&符号分隔的属性列表。这些属性使您能够指示 MySQL Connector/J 执行各种任务。

查看[*MySQL 参考手册*](https://dev.mysql.com/doc/) 获取更多信息。
