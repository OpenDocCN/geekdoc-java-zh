# 不再需要 JDBC Class.forName()

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-class-forname-is-no-longer-required/>

自从 Java 1.6，JDBC 4.0 API，它提供了自动发现`java.sql.Driver`的新特性，这意味着不再需要`Class.forName`。只需将任何 JDBC 4.x 驱动程序放到项目类路径中，Java 就能够检测到它。

例如，PostgreSQL 的 JDBC 驱动程序:

pom.xml

```java
 <dependency>
		<groupId>org.postgresql</groupId>
		<artifactId>postgresql</artifactId>
		<version>42.2.5</version>
	</dependency> 
```

这很有效:

```java
 package com.mkyong.jdbc;

import java.sql.*;

public class JDBCExample {

    public static void main(String[] args) {

        try {

            // this is optional @since 1.6
            // Class.forName("org.postgresql.Driver");

            // auto close connection
            try (Connection conn =
                         DriverManager.getConnection("jdbc:postgresql://127.0.0.1:5432/test",
                                 "postgres", "password")) {

                Statement statement = conn.createStatement();
                //...

            }

        } catch (Exception e) {
            System.err.println("Something went wrong!");
            e.printStackTrace();
        }

    }

} 
```

## 参考

*   [滚动到 JDBC 章节](http://web.archive.org/web/20230101144945/https://docs.oracle.com/javase/8/docs/api/java/sql/package-summary.html#package.description)
*   [驱动管理器 JavaDocs](http://web.archive.org/web/20230101144945/https://docs.oracle.com/javase/6/docs/api/java/sql/DriverManager.html)

<input type="hidden" id="mkyong-current-postId" value="15107">