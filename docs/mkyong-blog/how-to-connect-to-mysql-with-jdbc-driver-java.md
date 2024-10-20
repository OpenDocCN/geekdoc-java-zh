# 用 JDBC 驱动程序连接到 MySQL

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/how-to-connect-to-mysql-with-jdbc-driver-java/>

一个 JDBC 的例子，展示了如何用 JDBC 驱动程序连接到 MySQL 数据库。

*测试者:*

*   Java 8
*   MySQL 5.7
*   MySQL JDBC 驱动`mysql-connector-java:8.0.16`

## 1.下载 MySQL JDBC 驱动程序

访问 https://dev.mysql.com/downloads/connector/j/下载最新的 MySQL JDBC 驱动程序。

![mysql-connector-j](img/eab35b88ba0a965ea6eb017a849c269c.png)

## 2.JDBC 连接

2.1 建立到 MySQL 数据库的连接。

JDBCExample.java

```java
 import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class JDBCExample {

    public static void main(String[] args) {

        // https://docs.oracle.com/javase/8/docs/api/java/sql/package-summary.html#package.description
        // auto java.sql.Driver discovery -- no longer need to load a java.sql.Driver class via Class.forName

        // register JDBC driver, optional since java 1.6
        /*try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }*/

        // auto close connection
        try (Connection conn = DriverManager.getConnection(
                "jdbc:mysql://127.0.0.1:3306/test", "root", "password")) {

            if (conn != null) {
                System.out.println("Connected to the database!");
            } else {
                System.out.println("Failed to make connection!");
            }

        } catch (SQLException e) {
            System.err.format("SQL State: %s\n%s", e.getSQLState(), e.getMessage());
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
} 
```

测试:

```java
 # compile
> javac JDBCExample.java

# run
> java JDBCExample

SQL State: 08001
No suitable driver found for jdbc:mysql://127.0.0.1:3306/test 
```

要用`java`命令运行它，我们需要手动加载 MySQL JDBC 驱动程序。假设所有东西都存储在`c:\test`文件夹中，用这个`-cp`选项再次运行它。

![project layout](img/647d7177f8487f01aa0d9a29fe9162a0.png)

```java
 > java -cp "c:\test\mysql-connector-java-8.0.16.jar;c:\test" JDBCExample
Connected to the database! 
```

## 3.Maven 项目

3.1 MySQL JDBC 驱动程序可以在 Maven 中央存储库中获得。

pom.xml

```java
 <dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.16</version>
    </dependency> 
```

3.2 一个简单的 JDBC 选择示例。

JDBCExample2.java

```java
 import com.mkyong.jdbc.model.Employee;

import java.math.BigDecimal;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class JDBCExample2 {

    public static void main(String[] args) {

        System.out.println("MySQL JDBC Connection Testing ~");

        List<Employee> result = new ArrayList<>();

        String SQL_SELECT = "Select * from EMPLOYEE";

        try (Connection conn = DriverManager.getConnection(
                "jdbc:mysql://127.0.0.1:3306/test", "root", "password");
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

                result.add(obj);

            }
            result.forEach(x -> System.out.println(x));

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
    ID INT NOT NULL AUTO_INCREMENT,
    NAME VARCHAR(100) NOT NULL,
    SALARY DECIMAL(15, 2) NOT NULL,
    CREATED_DATE DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (ID)
); 
```

## 下载源代码

$ git clone [https://github.com/mkyong/java-jdbc.git](http://web.archive.org/web/20210502132239/https://github.com/mkyong/java-jdbc.git)

## 参考

*   [MySQL 数据库](http://web.archive.org/web/20210502132239/https://dev.mysql.com/downloads/mysql/)
*   [MySQL GUI–工作台](http://web.archive.org/web/20210502132239/https://dev.mysql.com/downloads/workbench/)
*   [Java JDBC 教程](/web/20210502132239/https://mkyong.com/tutorials/jdbc-tutorials/)

Tags : [jdbc](http://web.archive.org/web/20210502132239/https://mkyong.com/tag/jdbc/) [mysql](http://web.archive.org/web/20210502132239/https://mkyong.com/tag/mysql/)<input type="hidden" id="mkyong-current-postId" value="1634">