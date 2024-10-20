# JDBC 准备报表–选择行列表

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-preparestatement-example-select-list-of-the-records/>

一个从数据库中选择一列行的 JDBC `PreparedStatement`例子。

RowSelect.java

```java
 package com.mkyong.jdbc.preparestatement.row;

import com.mkyong.jdbc.model.Employee;

import java.math.BigDecimal;
import java.sql.*;

public class RowSelect {

    private static final String SQL_SELECT = "SELECT * FROM EMPLOYEE";

    public static void main(String[] args) {

        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://127.0.0.1:5432/test", "postgres", "password");
             PreparedStatement preparedStatement = conn.prepareStatement(SQL_SELECT)) {

            ResultSet resultSet = preparedStatement.executeQuery();

            while (resultSet.next()) {

                long id = resultSet.getLong("ID");
                String name = resultSet.getString("NAME");
                BigDecimal salary = resultSet.getBigDecimal("SALARY");
                Timestamp createdDate = resultSet.getTimestamp("CREATED_DATE");

                Employee obj = new Employee();
                obj.setId(id);
                obj.setName(name);
                obj.setSalary(salary);
                // Timestamp -> LocalDateTime
                obj.setCreatedDate(createdDate.toLocalDateTime());

                System.out.println(obj);
            }

        } catch (SQLException e) {
            System.err.format("SQL State: %s\n%s", e.getSQLState(), e.getMessage());
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

} 
```

Employee.java

```java
 package com.mkyong.jdbc.model;

import java.math.BigDecimal;
import java.time.LocalDateTime;

public class Employee {

    private Long id;
    private String name;
    private BigDecimal salary;
    private LocalDateTime createdDate;

    //...
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

$ git clone [https://github.com/mkyong/java-jdbc.git](http://web.archive.org/web/20210506143048/https://github.com/mkyong/java-jdbc.git)

## 参考

*   [PostgreSQL–选择](http://web.archive.org/web/20210506143048/https://www.postgresql.org/docs/11/sql-select.html)
*   [结果集 JavaDocs](http://web.archive.org/web/20210506143048/https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSet.html)
*   [prepared statement JavaDocs](http://web.archive.org/web/20210506143048/https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html)
*   [Java JDBC 教程](/web/20210506143048/https://mkyong.com/tutorials/jdbc-tutorials/)

Tags : [jdbc](http://web.archive.org/web/20210506143048/https://mkyong.com/tag/jdbc/) [preparedstatement](http://web.archive.org/web/20210506143048/https://mkyong.com/tag/preparedstatement/) [select](http://web.archive.org/web/20210506143048/https://mkyong.com/tag/select/)<input type="hidden" id="mkyong-current-postId" value="8275">