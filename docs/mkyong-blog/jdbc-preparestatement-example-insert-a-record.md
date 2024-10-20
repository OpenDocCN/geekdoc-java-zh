# JDBC 准备报表–插入一行

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-preparestatement-example-insert-a-record/>

一个 JDBC `PreparedStatement`向数据库中插入一行的例子。

RowInsert.java

```java
 package com.mkyong.jdbc.preparestatement.row;

import java.math.BigDecimal;
import java.sql.*;
import java.time.LocalDateTime;

public class RowInsert {

    private static final String SQL_INSERT = "INSERT INTO EMPLOYEE (NAME, SALARY, CREATED_DATE) VALUES (?,?,?)";

    public static void main(String[] args) {

        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://127.0.0.1:5432/test", "postgres", "password");
             PreparedStatement preparedStatement = conn.prepareStatement(SQL_INSERT)) {

            preparedStatement.setString(1, "mkyong");
            preparedStatement.setBigDecimal(2, new BigDecimal(799.88));
            preparedStatement.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));

            int row = preparedStatement.executeUpdate();

            // rows affected
            System.out.println(row); //1

        } catch (SQLException e) {
            System.err.format("SQL State: %s\n%s", e.getSQLState(), e.getMessage());
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

} 
```

表定义。

```java
 CREATE TABLE EMPLOYEE
(
    ID serial,
    NAME varchar(100) NOT NULL,
    SALARY numeric(15, 2) NOT NULL,
    CREATED_DATE timestamp with time zone NOT NULL DEFAULT CURRENT_TIMESTAMP
    PRIMARY KEY (ID)
); 
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

$ git clone [https://github.com/mkyong/java-jdbc.git](http://web.archive.org/web/20220120163144/https://github.com/mkyong/java-jdbc.git)

## 参考

*   [PostgreSQL–插入](http://web.archive.org/web/20220120163144/https://www.postgresql.org/docs/11/sql-insert.html)
*   [prepared statement JavaDocs](http://web.archive.org/web/20220120163144/https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html)
*   [Java JDBC 教程](/web/20220120163144/https://mkyong.com/tutorials/jdbc-tutorials/)

<input type="hidden" id="mkyong-current-postId" value="8261">