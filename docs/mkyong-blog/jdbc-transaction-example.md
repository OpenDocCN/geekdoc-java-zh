# JDBC 交易示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-transaction-example/>

JDBC 事务确保一组 SQL 语句作为一个单元执行，要么成功执行所有语句，要么不执行任何语句(回滚所有更改)。

## 1.没有 JDBC 交易

1.1 插入两行并更新一行的 JDBC 示例。

TransactionExample.java

```java
 package com.mkyong.jdbc;

import java.math.BigDecimal;
import java.sql.*;
import java.time.LocalDateTime;

public class TransactionExample {

    public static void main(String[] args) {

        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://127.0.0.1:5432/test", "postgres", "password");
             Statement statement = conn.createStatement();
             PreparedStatement psInsert = conn.prepareStatement(SQL_INSERT);
             PreparedStatement psUpdate = conn.prepareStatement(SQL_UPDATE)) {

            statement.execute(SQL_TABLE_DROP);
            statement.execute(SQL_TABLE_CREATE);

            // Run list of insert commands
            psInsert.setString(1, "mkyong");
            psInsert.setBigDecimal(2, new BigDecimal(10));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.execute();

            psInsert.setString(1, "kungfu");
            psInsert.setBigDecimal(2, new BigDecimal(20));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.execute();

            // Run list of update commands

            // below line caused error, test transaction
            // org.postgresql.util.PSQLException: No value specified for parameter 1.
            psUpdate.setBigDecimal(2, new BigDecimal(999.99));

			//psUpdate.setBigDecimal(1, new BigDecimal(999.99));
            psUpdate.setString(2, "mkyong");
            psUpdate.execute();

        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    private static final String SQL_INSERT = "INSERT INTO EMPLOYEE (NAME, SALARY, CREATED_DATE) VALUES (?,?,?)";

    private static final String SQL_UPDATE = "UPDATE EMPLOYEE SET SALARY=? WHERE NAME=?";

    private static final String SQL_TABLE_CREATE = "CREATE TABLE EMPLOYEE"
            + "("
            + " ID serial,"
            + " NAME varchar(100) NOT NULL,"
            + " SALARY numeric(15, 2) NOT NULL,"
            + " CREATED_DATE timestamp with time zone NOT NULL DEFAULT CURRENT_TIMESTAMP,"
            + " PRIMARY KEY (ID)"
            + ")";

    private static final String SQL_TABLE_DROP = "DROP TABLE EMPLOYEE";

} 
```

输出，更新失败，并抛出一个异常，最后，插入 2 行，但更新被跳过。

```java
 org.postgresql.util.PSQLException: No value specified for parameter 1.
	at org.postgresql.core.v3.SimpleParameterList.checkAllParametersSet(SimpleParameterList.java:257)
	at org.postgresql.core.v3.QueryExecutorImpl.execute(QueryExecutorImpl.java:292)
	at org.postgresql.jdbc.PgStatement.executeInternal(PgStatement.java:441)
	at org.postgresql.jdbc.PgStatement.execute(PgStatement.java:365)
	at org.postgresql.jdbc.PgPreparedStatement.executeWithFlags(PgPreparedStatement.java:143)
	at org.postgresql.jdbc.PgPreparedStatement.execute(PgPreparedStatement.java:132)
	at com.mkyong.jdbc.TransactionExample.main(TransactionExample.java:41) 
```

![output](img/8e0619d228241d9e394857400cd9e983.png)

## 2.使用 JDBC 交易

2.1 要启用事务，请将自动提交设置为 false。

```java
 conn.setAutoCommit(false); // default true
// start transaction block

// insert
// update 
// if any errors within the start and end block, 
// rolled back all changes, none of the statements are executed.

// end transaction block
conn.commit(); 
```

2.2 JDBC 交易的例子相同。

TransactionExample.java

```java
 package com.mkyong.jdbc;

import java.math.BigDecimal;
import java.sql.*;
import java.time.LocalDateTime;

public class TransactionExample {

    public static void main(String[] args) {

        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://127.0.0.1:5432/test", "postgres", "password");
             Statement statement = conn.createStatement();
             PreparedStatement psInsert = conn.prepareStatement(SQL_INSERT);
             PreparedStatement psUpdate = conn.prepareStatement(SQL_UPDATE)) {

            statement.execute(SQL_TABLE_DROP);
            statement.execute(SQL_TABLE_CREATE);

            // start transaction block
            conn.setAutoCommit(false); // default true

            // Run list of insert commands
            psInsert.setString(1, "mkyong");
            psInsert.setBigDecimal(2, new BigDecimal(10));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.execute();

            psInsert.setString(1, "kungfu");
            psInsert.setBigDecimal(2, new BigDecimal(20));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.execute();

            // Run list of update commands

            // error, test roolback
            // org.postgresql.util.PSQLException: No value specified for parameter 1.
            psUpdate.setBigDecimal(2, new BigDecimal(999.99));
            //psUpdate.setBigDecimal(1, new BigDecimal(999.99));
            psUpdate.setString(2, "mkyong");
            psUpdate.execute();

            // end transaction block, commit changes
            conn.commit();

            // good practice to set it back to default true
            conn.setAutoCommit(true);

        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    //...

} 
```

输出，则不执行任何语句，insert 语句将回滚。

```java
 org.postgresql.util.PSQLException: No value specified for parameter 1.
	at org.postgresql.core.v3.SimpleParameterList.checkAllParametersSet(SimpleParameterList.java:257)
	at org.postgresql.core.v3.QueryExecutorImpl.execute(QueryExecutorImpl.java:292)
	at org.postgresql.jdbc.PgStatement.executeInternal(PgStatement.java:441)
	at org.postgresql.jdbc.PgStatement.execute(PgStatement.java:365)
	at org.postgresql.jdbc.PgPreparedStatement.executeWithFlags(PgPreparedStatement.java:143)
	at org.postgresql.jdbc.PgPreparedStatement.execute(PgPreparedStatement.java:132)
	at com.mkyong.jdbc.TransactionExample.main(TransactionExample.java:41) 
```

![output](img/94bbd55fefa5cff78a61e45a73fd6793.png)

## 3.额外…

修复`parameter 1`错误并查看预期结果。

```java
 //psUpdate.setBigDecimal(2, new BigDecimal(999.99));

	psUpdate.setBigDecimal(1, new BigDecimal(999.99));
	psUpdate.setString(2, "mkyong");
	psUpdate.execute(); 
```

输出

```java
 2 rows are inserted and 1 row is updated. 
```

![output](img/0d49dd0abc94ba649154393a70876fb1.png)

## 下载源代码

$ git clone [https://github.com/mkyong/java-jdbc.git](http://web.archive.org/web/20210817084911/https://github.com/mkyong/java-jdbc.git)
$ cd postgresql

## 参考

*   [使用交易](http://web.archive.org/web/20210817084911/https://docs.oracle.com/javase/tutorial/jdbc/basics/transactions.html)
*   [Java JDBC 教程](/web/20210817084911/https://mkyong.com/tutorials/jdbc-tutorials/)
*   [JDBC 准备报表-批量更新](/web/20210817084911/https://mkyong.com/jdbc/jdbc-preparedstatement-example-batch-update/)
*   [Spring JDBC template batch update()示例](/web/20210817084911/https://mkyong.com/spring/spring-jdbctemplate-batchupdate-example/)

Tags : [jdbc](http://web.archive.org/web/20210817084911/https://mkyong.com/tag/jdbc/) [postgresql](http://web.archive.org/web/20210817084911/https://mkyong.com/tag/postgresql/) [transaction](http://web.archive.org/web/20210817084911/https://mkyong.com/tag/transaction/)<input type="hidden" id="mkyong-current-postId" value="8375">