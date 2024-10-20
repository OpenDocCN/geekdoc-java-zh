# Java JDBC 教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/jdbc-tutorials/>

![jdbc logo](img/11c0370aefbf231180dc433f654f7055.png)

[Java 数据库连接(JDBC)](http://web.archive.org/web/20221230032439/https://en.wikipedia.org/wiki/Java_Database_Connectivity) API 使 Java 应用程序能够与数据库进行交互。

### 1.入门指南

*   [JDBC +甲骨文数据库](/web/20221230032439/https://mkyong.com/jdbc/connect-to-oracle-db-via-jdbc-driver-java/)
*   [JDBC + MySQL 数据库](/web/20221230032439/https://mkyong.com/jdbc/how-to-connect-to-mysql-with-jdbc-driver-java/)
*   [JDBC + PostgreSQL 数据库](/web/20221230032439/https://mkyong.com/jdbc/how-do-connect-to-postgresql-with-jdbc-driver-java/)

### 2.声明

这个`Statement`没有缓存，适用于简单和静态的 SQL 语句，比如 CREATE 或 DROP。在`Statement`中，我们在 SQL 中构造条件或参数的方式容易出现 SQL 注入，记住要避开引号和特殊字符。

*   `statement.execute(sql)`–通常用于 DDL，如创建或删除
*   `statement.executeUpdate(sql)`–通常用于 DML，如插入、更新、删除
*   `statement.executeQuery(sql)`–运行选择查询并返回一个`ResultSet`
*   `statement.executeBatch()`–批量运行 SQL 命令

文章:

*   [JDBC 语句–创建表格](/web/20221230032439/https://mkyong.com/jdbc/jdbc-statement-example-create-a-table/)
*   [JDBC 声明–插入一行](/web/20221230032439/https://mkyong.com/jdbc/jdbc-statement-example-insert-a-record/)
*   [JDBC 语句–更新一行](/web/20221230032439/https://mkyong.com/jdbc/jdbc-statement-example-update-a-record/)
*   [JDBC 声明–删除一行](/web/20221230032439/https://mkyong.com/jdbc/jdbc-statement-example-delete-a-record/)
*   [JDBC 语句–选择行列表](/web/20221230032439/https://mkyong.com/jdbc/jdbc-statement-example-select-list-of-the-records/)
*   [JDBC 声明–批量更新](/web/20221230032439/https://mkyong.com/jdbc/jdbc-statement-example-batch-update/)

### 3.准备报表

`PreparedStatement`扩展了`Statement`,通过预编译和缓存 SQL 语句来提供更好的性能，适用于需要多次执行的 SQL 语句。此外，它提供了许多`setXxx()`来通过转义引号和特殊字符来保护 SQL 注入。

*   `preparedStatement.execute()`–通常用于 DDL，如创建或删除
*   `preparedStatement.executeUpdate()`–通常用于 DML，如插入、更新、删除
*   `preparedStatement.executeQuery()`–运行选择查询并返回一个`ResultSet`
*   `preparedStatement.executeBatch()`–批量运行 SQL 命令

文章:

*   [JDBC 准备报表–创建表格](/web/20221230032439/https://mkyong.com/jdbc/jdbc-preparestatement-example-create-a-table/)
*   [JDBC 准备报表–插入一行](/web/20221230032439/https://mkyong.com/jdbc/jdbc-preparestatement-example-insert-a-record/)
*   [JDBC 准备报表–更新一行](/web/20221230032439/https://mkyong.com/jdbc/jdbc-preparestatement-example-update-a-record/)
*   [JDBC 准备报表–删除一行](/web/20221230032439/https://mkyong.com/jdbc/jdbc-preparestatement-example-delete-a-record/)
*   [JDBC 准备报表–选择行列表](/web/20221230032439/https://mkyong.com/jdbc/jdbc-preparestatement-example-select-list-of-the-records/)
*   [JDBC 准备报表-批量更新](/web/20221230032439/https://mkyong.com/jdbc/jdbc-preparedstatement-example-batch-update/)
*   [JDBC 在条件](/web/20221230032439/https://mkyong.com/jdbc/jdbc-preparedstatement-sql-in-condition/)下准备了一条语句 SQL

### 4.可调用语句

`CallableStatement`扩展了`PreparedStatement`，用于执行数据库中的存储过程或函数。

*   `conn.prepareCall(sql)`

**甲骨文数据库**

*   [参数示例中的 JDBC 可调用语句-存储过程](/web/20221230032439/https://mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-in-parameter-example/)
*   [JDBC 可调用语句-存储过程输出参数示例](/web/20221230032439/https://mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-out-parameter-example/)
*   [JDBC 可调用语句-存储过程光标示例](/web/20221230032439/https://mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-cursor-example/)

**PostgreSQL**

*   [JDBC 可调用语句-存储函数](/web/20221230032439/https://mkyong.com/jdbc/jdbc-callablestatement-postgresql-stored-function/)

### 5.交易

*   [JDBC 交易示例](/web/20221230032439/https://mkyong.com/jdbc/jdbc-transaction-example/)

```java
 conn.setAutoCommit(false); // default true
// start transaction block

// SQL statements

// end transaction block
conn.commit();
conn.setAutoCommit(true); 
```

### 6.Spring JDBC 数据库访问

`JdbcTemplate`例子。

*   [Spring Boot JDBC 的例子](/web/20221230032439/https://mkyong.com/spring-boot/spring-boot-jdbc-examples/)
*   [Spring Boot JDBC 存储过程示例](/web/20221230032439/https://mkyong.com/spring-boot/spring-boot-jdbc-stored-procedure-examples/)
*   [Spring JdbcTemplate 查询示例](/web/20221230032439/https://mkyong.com/spring/spring-jdbctemplate-querying-examples/)
*   [Spring JDBC template Handle Large ResultSet](/web/20221230032439/https://mkyong.com/spring/spring-jdbctemplate-handle-large-resultset/)
*   [Spring JDBC template batch update()示例](/web/20221230032439/https://mkyong.com/spring/spring-jdbctemplate-batchupdate-example/)
*   [Spring Boot JDBC 图像斑点示例](/web/20221230032439/https://mkyong.com/spring-boot/spring-jdbctemplate-save-image-into-database-blob-examples/)

### 常见问题

*   [如何在您的 Maven 本地存储库中添加 Oracle JDBC 驱动程序](/web/20221230032439/https://mkyong.com/maven/how-to-add-oracle-jdbc-driver-in-your-maven-local-repository/)
*   JDBC——如何打印数据库中的所有表名？
*   [JDBC Class.forName()不再需要](/web/20221230032439/https://mkyong.com/jdbc/jdbc-class-forname-is-no-longer-required/)
*   [Oracle–ORA-12505，TNS:监听器当前不知道连接描述符](/web/20221230032439/https://mkyong.com/jdbc/ora-12505-tnslistener-does-not-currently-know-of-sid-given-in-connect-descriptor/)中给定的 SID
*   [java.sql.SQLException:不允许操作:序号绑定和命名绑定不能组合！](/web/20221230032439/https://mkyong.com/jdbc/java-sql-sqlexception-operation-not-allowed-ordinal-binding-and-named-binding-cannot-be-combined/)
*   [java.sql.SQLException:服务器时区值“xx time”无法识别](/web/20221230032439/https://mkyong.com/jdbc/java-sql-sqlexception-the-server-time-zone-value-xx-time-is-unrecognized/)

### 参考

*   [甲骨文-JDBC 教程](http://web.archive.org/web/20221230032439/https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html)
*   [Java 8 中的 SQL:结果集流](http://web.archive.org/web/20221230032439/https://www.jooq.org/java-8-and-sql)
*   [维基百科–Java 数据库连接](http://web.archive.org/web/20221230032439/https://en.wikipedia.org/wiki/Java_Database_Connectivity)
*   [维基百科–SQL 注入](http://web.archive.org/web/20221230032439/https://en.wikipedia.org/wiki/SQL_injection)
*   [语句 JavaDocs](http://web.archive.org/web/20221230032439/https://docs.oracle.com/javase/10/docs/api/java/sql/Statement.html)
*   [prepared statement JavaDocs](http://web.archive.org/web/20221230032439/https://docs.oracle.com/javase/10/docs/api/java/sql/PreparedStatement.html)
*   [可调用声明 JavaDocs](http://web.archive.org/web/20221230032439/https://docs.oracle.com/javase/10/docs/api/java/sql/CallableStatement.html)
*   [java.sql 摘要 JaavDocs](http://web.archive.org/web/20221230032439/https://docs.oracle.com/javase/8/docs/api/java/sql/package-summary.html#package.description)

**甲骨文**

*   [甲骨文数据库](http://web.archive.org/web/20221230032439/https://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html)
*   [Oracle GUI–SQL 开发人员](http://web.archive.org/web/20221230032439/https://www.oracle.com/database/technologies/appdev/sql-developer.html)

**MySQL** 的实现

*   [MySQL 数据库](http://web.archive.org/web/20221230032439/https://dev.mysql.com/downloads/mysql/)
*   [MySQL GUI–工作台](http://web.archive.org/web/20221230032439/https://dev.mysql.com/downloads/workbench/)

**PostgreSQL**

*   [PostgreSQL 数据库](http://web.archive.org/web/20221230032439/https://www.postgresql.org/)
*   [PostgreSQL GUI–pgAdmin](http://web.archive.org/web/20221230032439/https://www.pgadmin.org/)

<input type="hidden" id="mkyong-current-postId" value="8380">