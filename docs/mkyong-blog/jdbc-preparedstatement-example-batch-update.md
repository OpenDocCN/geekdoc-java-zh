# JDBC 编制的报表-批量更新

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-preparedstatement-example-batch-update/>

一个 JDBC `PreparedStatement`例子向数据库发送一批 SQL 命令(创建、插入、更新)。

BatchUpdate.java

```java
 package com.mkyong.jdbc.preparestatement;

import java.math.BigDecimal;
import java.sql.*;
import java.time.LocalDateTime;
import java.util.Arrays;

public class BatchUpdate {

    public static void main(String[] args) {

        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://127.0.0.1:5432/test", "postgres", "password");
             PreparedStatement psDDL = conn.prepareStatement(SQL_CREATE);
             PreparedStatement psInsert = conn.prepareStatement(SQL_INSERT);
             PreparedStatement psUpdate = conn.prepareStatement(SQL_UPDATE)) {

            // commit all or rollback all, if any errors
            conn.setAutoCommit(false); // default true

            psDDL.execute();

            // Run list of insert commands
            psInsert.setString(1, "mkyong");
            psInsert.setBigDecimal(2, new BigDecimal(10));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.addBatch();

            psInsert.setString(1, "kungfu");
            psInsert.setBigDecimal(2, new BigDecimal(20));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.addBatch();

            psInsert.setString(1, "james");
            psInsert.setBigDecimal(2, new BigDecimal(30));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.addBatch();

            int[] rows = psInsert.executeBatch();

            System.out.println(Arrays.toString(rows));

            // Run list of update commands
            psUpdate.setBigDecimal(1, new BigDecimal(999.99));
            psUpdate.setString(2, "mkyong");
            psUpdate.addBatch();

            psUpdate.setBigDecimal(1, new BigDecimal(888.88));
            psUpdate.setString(2, "james");
            psUpdate.addBatch();

            int[] rows2 = psUpdate.executeBatch();

            System.out.println(Arrays.toString(rows2));

            conn.commit();

        } catch (SQLException e) {
            System.err.format("SQL State: %s\n%s", e.getSQLState(), e.getMessage());
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    private static final String SQL_INSERT = "INSERT INTO EMPLOYEE (NAME, SALARY, CREATED_DATE) VALUES (?,?,?)";

    private static final String SQL_UPDATE = "UPDATE EMPLOYEE SET SALARY=? WHERE NAME=?";

    private static final String SQL_CREATE = "CREATE TABLE EMPLOYEE"
            + "("
            + " ID serial,"
            + " NAME varchar(100) NOT NULL,"
            + " SALARY numeric(15, 2) NOT NULL,"
            + " CREATED_DATE timestamp with time zone NOT NULL DEFAULT CURRENT_TIMESTAMP,"
            + " PRIMARY KEY (ID)"
            + ")";

} 
```

这个批处理示例使用了事务，如果出现错误，要么全部提交，要么全部回滚。

```java
 conn.setAutoCommit(false);

	// SQL 

	conn.commit(); 
```

*PS 用 PostgreSQL 11 和 Java 8 测试*

pom.xml

```java
 <dependency>
		<groupId>org.postgresql</groupId>
		<artifactId>postgresql</artifactId>
		<version>42.2.5</version>
	</dependency> 
```

## 下载源代码

$ git clone [https://github.com/mkyong/java-jdbc.git](http://web.archive.org/web/20220618222636/https://github.com/mkyong/java-jdbc.git)

## 参考

*   [JDBC 声明–批量更新](/web/20220618222636/https://mkyong.com/jdbc/jdbc-statement-example-batch-update/)
*   [prepared statement JavaDocs](http://web.archive.org/web/20220618222636/https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html)
*   [Java JDBC 教程](/web/20220618222636/https://mkyong.com/tutorials/jdbc-tutorials/)
*   [PostgreSQL–插入](http://web.archive.org/web/20220618222636/https://www.postgresql.org/docs/11/sql-insert.html)

<input type="hidden" id="mkyong-current-postId" value="8361">