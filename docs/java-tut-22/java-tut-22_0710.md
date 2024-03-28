# 教程：JDBC 简介

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/overview/index.html`](https://docs.oracle.com/javase/tutorial/jdbc/overview/index.html)

JDBC API 是一个可以访问任何类型表格数据的 Java API，特别是存储在关系数据库中的数据。

JDBC 帮助您编写管理这三种编程活动的 Java 应用程序：

1.  连接到数据源，比如数据库

1.  向数据库发送查询和更新语句

1.  从数据库中检索并处理查询结果

以下简单的代码片段给出了这三个步骤的简单示例：

```java
public void connectToAndQueryDatabase(String username, String password) {

    Connection con = DriverManager.getConnection(
                         "jdbc:myDriver:myDatabase",
                         username,
                         password);

    Statement stmt = con.createStatement();
    ResultSet rs = stmt.executeQuery("SELECT a, b, c FROM Table1");

    while (rs.next()) {
        int x = rs.getInt("a");
        String s = rs.getString("b");
        float f = rs.getFloat("c");
    }
}

```

这段简短的代码片段实例化了一个`DriverManager`对象，用于连接数据库驱动程序并登录数据库，实例化了一个`Statement`对象，将您的 SQL 语言查询传递给数据库；实例化了一个`ResultSet`对象，检索您的查询结果，并执行一个简单的`while`循环，用于检索和显示这些结果。就是这么简单。

## JDBC 产品组件

JDBC 包括四个组件：

1.  **JDBC API** —  JDBC™ API 提供了从 Java™编程语言对关系数据进行编程访问的方式。使用 JDBC API，应用程序可以执行 SQL 语句，检索结果，并将更改传播回底层数据源。JDBC API 还可以在分布式、异构环境中与多个数据源交互。

    JDBC API 是 Java 平台的一部分，包括*Java™标准版*（Java™ SE）和*Java™企业版*（Java™ EE）。JDBC 4.0 API 分为两个包：`java.sql`和`javax.sql.` 这两个包都包含在 Java SE 和 Java EE 平台中。

1.  **JDBC 驱动管理器** —  JDBC `DriverManager`类定义了可以将 Java 应用程序连接到 JDBC 驱动程序的对象。`DriverManager`一直是 JDBC 架构的支柱。它非常小而简单。

    标准扩展包`javax.naming`和`javax.sql`允许您使用注册到*Java 命名和目录接口*™（JNDI）命名服务的`DataSource`对象与数据源建立连接。您可以使用任一连接机制，但尽可能使用`DataSource`对象是推荐的。

1.  **JDBC 测试套件** —  JDBC 驱动程序测试套件帮助您确定 JDBC 驱动程序是否能运行您的程序。这些测试并不全面或详尽，但它们确实涵盖了 JDBC API 中的许多重要特性。

1.  **JDBC-ODBC 桥** —  Java 软件桥通过 ODBC 驱动程序提供 JDBC 访问。请注意，您需要将 ODBC 二进制代码加载到每台使用此驱动程序的客户端机器上。因此，ODBC 驱动程序最适合用于企业网络，其中客户端安装不是一个主要问题，或者用于在 Java 中编写的应用程序服务器代码的三层架构。

这个教程使用这四个 JDBC 组件中的前两个来连接数据库，然后构建一个使用 SQL 命令与测试关系数据库通信的 Java 程序。最后两个组件用于在专业环境中测试 Web 应用程序，或与支持 ODBC 的数据库管理系统通信。
