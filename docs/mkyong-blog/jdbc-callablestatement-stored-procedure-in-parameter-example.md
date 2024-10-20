# JDBC 可调用语句–参数示例中的存储过程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-in-parameter-example/>

代码片段向您展示了如何通过 JDBC `CallableStatement`调用 Oracle 存储过程，以及如何从 Java 向存储过程传递参数。

```java
 //insertDBUSER is stored procedure
String insertStoreProc = "{call insertDBUSER(?,?,?,?)}";
callableStatement = dbConnection.prepareCall(insertStoreProc);
callableStatement.setInt(1, 1000);
callableStatement.setString(2, "mkyong");
callableStatement.setString(3, "system");
callableStatement.setDate(4, getCurrentDate());
callableStatement.executeUpdate(); 
```

## JDBC 可调用语句示例

参见完整的 JDBC `CallableStatement`例子。

 ## 1.存储过程

Oracle 数据库中的存储过程。后来，称之为经 JDBC。

```java
 CREATE OR REPLACE PROCEDURE insertDBUSER(
	   p_userid IN DBUSER.USER_ID%TYPE,
	   p_username IN DBUSER.USERNAME%TYPE,
	   p_createdby IN DBUSER.CREATED_BY%TYPE,
	   p_date IN DBUSER.CREATED_DATE%TYPE)
IS
BEGIN

  INSERT INTO DBUSER ("USER_ID", "USERNAME", "CREATED_BY", "CREATED_DATE") 
  VALUES (p_userid, p_username,p_createdby, p_date);

  COMMIT;

END;
/ 
```

 ## 2.通过 CallableStatement 调用存储过程

JDBC 通过`CallableStatement`调用存储过程的例子。

*文件:JDBCCallableStatementINParameterExample.java*

```java
 package com.mkyong.jdbc;

import java.sql.CallableStatement;
import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.SQLException;

public class JDBCCallableStatementINParameterExample {

	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
	private static final String DB_USER = "user";
	private static final String DB_PASSWORD = "password";

	public static void main(String[] argv) {

		try {

			callOracleStoredProcINParameter();

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		}

	}

	private static void callOracleStoredProcINParameter() throws SQLException {

		Connection dbConnection = null;
		CallableStatement callableStatement = null;

		String insertStoreProc = "{call insertDBUSER(?,?,?,?)}";

		try {
			dbConnection = getDBConnection();
			callableStatement = dbConnection.prepareCall(insertStoreProc);

			callableStatement.setInt(1, 1000);
			callableStatement.setString(2, "mkyong");
			callableStatement.setString(3, "system");
			callableStatement.setDate(4, getCurrentDate());

			// execute insertDBUSER store procedure
			callableStatement.executeUpdate();

			System.out.println("Record is inserted into DBUSER table!");

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		} finally {

			if (callableStatement != null) {
				callableStatement.close();
			}

			if (dbConnection != null) {
				dbConnection.close();
			}

		}

	}

	private static Connection getDBConnection() {

		Connection dbConnection = null;

		try {

			Class.forName(DB_DRIVER);

		} catch (ClassNotFoundException e) {

			System.out.println(e.getMessage());

		}

		try {

			dbConnection = DriverManager.getConnection(
				DB_CONNECTION, DB_USER,DB_PASSWORD);
			return dbConnection;

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		}

		return dbConnection;

	}

	private static java.sql.Date getCurrentDate() {
		java.util.Date today = new java.util.Date();
		return new java.sql.Date(today.getTime());
	}

} 
```

完成了。当执行上述 JDBC 示例时，将通过存储过程"`insertDBUSER`"向数据库中插入一条新记录。

## 参考

1.  [http://download . Oracle . com/javase/6/docs/API/Java/SQL/callable statement . html](http://web.archive.org/web/20190225104729/http://download.oracle.com/javase/6/docs/api/java/sql/CallableStatement.html)
2.  [http://docsrv . SCO . com/JDK _ guide/JDBC/getstart/callable statement . doc . html](http://web.archive.org/web/20190225104729/http://docsrv.sco.com/JDK_guide/jdbc/getstart/callablestatement.doc.html)
3.  [http://on Java . com/pub/a/on Java/2003/08/13/stored _ procedures . html](http://web.archive.org/web/20190225104729/http://onjava.com/pub/a/onjava/2003/08/13/stored_procedures.html)

[callablestatement](http://web.archive.org/web/20190225104729/http://www.mkyong.com/tag/callablestatement/) [jdbc](http://web.archive.org/web/20190225104729/http://www.mkyong.com/tag/jdbc/) [store procedure](http://web.archive.org/web/20190225104729/http://www.mkyong.com/tag/store-procedure/)







