# 课程：JDBC 基础知识

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/index.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html)

在这节课中，您将学习 JDBC API 的基础知识。

+   入门指南设置了一个基本的数据库开发环境，并展示了如何编译和运行 JDBC 教程示例。

+   使用 JDBC 处理 SQL 语句概述了处理任何 SQL 语句所需的步骤。接下来的页面将更详细地描述这些步骤：

    +   建立连接连接到您的数据库。

    +   连接到数据源对象展示了如何使用`DataSource`对象连接到数据库，这是获取数据源连接的首选方式。

    +   处理 SQLException 展示了如何处理由数据库错误引起的异常。

    +   设置表格描述了 JDBC 教程示例中使用的所有数据库表格以及如何使用 JDBC API 和 SQL 脚本创建和填充表格。

    +   从结果集中检索和修改值开发了配置数据库、发送查询和从数据库检索数据的过程。

    +   使用预编译语句描述了创建数据库查询的更灵活方式。

    +   使用事务展示了如何控制数据库查询何时实际执行。

+   使用 RowSet 对象介绍了`RowSet`对象；这些对象以一种比结果集更灵活且更易于使用的方式保存表格数据。接下来的页面将描述可用的不同类型的`RowSet`对象：

    +   使用 JdbcRowSet 对象

    +   使用 CachedRowSet 对象

    +   使用 JoinRowSet 对象

    +   使用 FilteredRowSet 对象

    +   使用 WebRowSet 对象

+   使用高级数据类型介绍了其他数据类型；接下来的页面将更详细地描述这些数据类型：

    +   使用大对象

    +   使用 SQLXML 对象

    +   使用数组对象

    +   使用 DISTINCT 数据类型

    +   使用结构化对象

    +   使用自定义类型映射

    +   使用数据链接对象

    +   使用行 ID 对象

+   使用存储过程展示了如何创建和使用存储过程，这是一组可以像调用 Java 方法一样具有可变输入和输出参数的 SQL 语句组。

+   使用 GUI API 的 JDBC 演示了如何将 JDBC 与 Swing API 集成。
