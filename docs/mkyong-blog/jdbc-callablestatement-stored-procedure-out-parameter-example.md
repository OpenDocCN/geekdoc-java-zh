# JDBC 可调用语句–存储过程输出参数示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-out-parameter-example/>

一个 JDBC `CallableStatement`例子来调用一个接受输入和输出参数的存储过程。

*使用 Java 8 和 Oracle database 19c 进行测试*

pom.xml

```java
 <dependency>
		<groupId>com.oracle</groupId>
		<artifactId>ojdbc</artifactId>
		<version>8</version>
		<scope>system</scope>
		<systemPath>path.to/ojdbc8.jar</systemPath>
	</dependency> 
```

## 1.JDBC 可赎回声明

1.1 接受 IN 和 OUT 参数的 PL/SQL 存储过程。

```java
 CREATE OR REPLACE PROCEDURE get_employee_by_id(
	   p_id IN EMPLOYEE.ID%TYPE,
       o_name OUT EMPLOYEE.NAME%TYPE,
	   o_salary OUT EMPLOYEE.SALARY%TYPE,
	   o_date OUT EMPLOYEE.CREATED_DATE%TYPE)
    AS
    BEGIN

    SELECT NAME , SALARY, CREATED_DATE INTO o_name, o_salary, o_date from EMPLOYEE WHERE ID = p_id;

    END; 
```

1.2 JDBC 调用上述存储过程的例子。

StoreProcedureOutParameter.java

```java
 package com.mkyong.jdbc.callablestatement;

import java.math.BigDecimal;
import java.sql.*;

public class StoreProcedureOutParameter {

    public static void main(String[] args) {

        String createSP = "CREATE OR REPLACE PROCEDURE get_employee_by_id( "
                + " p_id IN EMPLOYEE.ID%TYPE, "
                + " o_name OUT EMPLOYEE.NAME%TYPE, "
                + " o_salary OUT EMPLOYEE.SALARY%TYPE, "
                + " o_date OUT EMPLOYEE.CREATED_DATE%TYPE) "
                + " AS "
                + " BEGIN "
                + "     SELECT NAME, SALARY, CREATED_DATE INTO o_name, o_salary, o_date from EMPLOYEE WHERE ID = p_id; "
                + " END;";

        String runSP = "{ call get_employee_by_id(?,?,?,?) }";

        try (Connection conn = DriverManager.getConnection(
                "jdbc:oracle:thin:@localhost:1521:orcl", "system", "Password123");
             Statement statement = conn.createStatement();
             CallableStatement callableStatement = conn.prepareCall(runSP)) {

            // create or replace stored procedure
            statement.execute(createSP);

            callableStatement.setInt(1, 3);

            callableStatement.registerOutParameter(2, java.sql.Types.VARCHAR);
            callableStatement.registerOutParameter(3, Types.DECIMAL);
            callableStatement.registerOutParameter(4, java.sql.Types.DATE);

            // run it
            callableStatement.executeUpdate();

            // java.sql.SQLException: operation not allowed: Ordinal binding and Named binding cannot be combined!
            /*String name = callableStatement.getString("NAME");
            BigDecimal salary = callableStatement.getBigDecimal("SALARY");
            Timestamp createdDate = callableStatement.getTimestamp("CREATED_DATE");*/

            String name = callableStatement.getString(2);
            BigDecimal salary = callableStatement.getBigDecimal(3);
            Timestamp createdDate = callableStatement.getTimestamp(4);

            System.out.println("name: " + name);
            System.out.println("salary: " + salary);
            System.out.println("createdDate: " + createdDate);

        } catch (SQLException e) {
            System.err.format("SQL State: %s\n%s", e.getSQLState(), e.getMessage());
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
} 
```

## 下载源代码

$ git clone [https://github.com/mkyong/java-jdbc.git](http://web.archive.org/web/20191208171826/https://github.com/mkyong/java-jdbc.git)

## 参考

*   [Oracle 参考光标](http://web.archive.org/web/20191208171826/http://www.oradev.com/ref_cursor.jsp)
*   [可调用声明 JavaDocs](http://web.archive.org/web/20191208171826/https://docs.oracle.com/javase/8/docs/api/java/sql/CallableStatement.html)
*   [Java JDBC 教程](http://web.archive.org/web/20191208171826/https://www.mkyong.com/tutorials/jdbc-tutorials/)

[callablestatement](http://web.archive.org/web/20191208171826/https://www.mkyong.com/tag/callablestatement/) [jdbc](http://web.archive.org/web/20191208171826/https://www.mkyong.com/tag/jdbc/) [oracle](http://web.archive.org/web/20191208171826/https://www.mkyong.com/tag/oracle/) [stored procedure](http://web.archive.org/web/20191208171826/https://www.mkyong.com/tag/stored-procedure/)<input type="hidden" id="mkyong-current-postId" value="8328">







