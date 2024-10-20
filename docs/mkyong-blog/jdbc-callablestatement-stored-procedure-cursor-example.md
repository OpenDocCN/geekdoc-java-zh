# JDBC 可调用语句–存储过程游标示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-cursor-example/>

对于 Oracle 存储过程返回的游标参数，您可以

1.  通过 JDBC 注册`CallableStatement.registerOutParameter(index,OracleTypes.CURSOR)`。
2.  通过`callableStatement.getObject(index)`取回。

查看代码片段

```java
 //getDBUSERCursor is a stored procedure
String getDBUSERCursorSql = "{call getDBUSERCursor(?,?)}";
callableStatement = dbConnection.prepareCall(getDBUSERCursorSql);
callableStatement.setString(1, "mkyong");
callableStatement.registerOutParameter(2, OracleTypes.CURSOR);

// execute getDBUSERCursor store procedure
callableStatement.executeUpdate();

// get cursor and cast it to ResultSet
rs = (ResultSet) callableStatement.getObject(2);

// loop it like normal
while (rs.next()) {
	String userid = rs.getString("USER_ID");
	String userName = rs.getString("USERNAME");
} 
```

## JDBC 可调用语句光标示例

关于输出**光标**参数，参见完整的 JDBC `CallableStatement`示例。

 ## 1.存储过程

一个 Oracle 存储过程，有一个 IN 和一个 OUT 游标参数。后来，称之为经 JDBC。

```java
 CREATE OR REPLACE PROCEDURE getDBUSERCursor(
	   p_username IN DBUSER.USERNAME%TYPE,
	   c_dbuser OUT SYS_REFCURSOR)
IS
BEGIN

  OPEN c_dbuser FOR
  SELECT * FROM DBUSER WHERE USERNAME LIKE p_username || '%';

END;
/ 
```

 ## 2.通过 CallableStatement 调用存储过程

JDBC 示例调用上述存储过程，将返回的**光标**转换为**结果集**，并按顺序遍历记录。

*文件:JDBCCallableStatementCURSORExample.java*

```java
 package com.mkyong.jdbc;

import java.sql.CallableStatement;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;

import oracle.jdbc.OracleTypes;

public class JDBCCallableStatementCURSORExample {

	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
	private static final String DB_USER = "user";
	private static final String DB_PASSWORD = "password";

	public static void main(String[] argv) {

		try {

			callOracleStoredProcCURSORParameter();

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		}

	}

	private static void callOracleStoredProcCURSORParameter()
			throws SQLException {

		Connection dbConnection = null;
		CallableStatement callableStatement = null;
		ResultSet rs = null;

		String getDBUSERCursorSql = "{call getDBUSERCursor(?,?)}";

		try {
			dbConnection = getDBConnection();
			callableStatement = dbConnection.prepareCall(getDBUSERCursorSql);

			callableStatement.setString(1, "mkyong");
			callableStatement.registerOutParameter(2, OracleTypes.CURSOR);

			// execute getDBUSERCursor store procedure
			callableStatement.executeUpdate();

			// get cursor and cast it to ResultSet
			rs = (ResultSet) callableStatement.getObject(2);

			while (rs.next()) {
				String userid = rs.getString("USER_ID");
				String userName = rs.getString("USERNAME");
				String createdBy = rs.getString("CREATED_BY");
				String createdDate = rs.getString("CREATED_DATE");

				System.out.println("UserName : " + userid);
				System.out.println("UserName : " + userName);
				System.out.println("CreatedBy : " + createdBy);
				System.out.println("CreatedDate : " + createdDate);
			}

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		} finally {

			if (rs != null) {
				rs.close();
			}

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

} 
```

完成了。

## 参考

1.  [http://www.oradev.com/ref_cursor.jsp](http://web.archive.org/web/20190224154700/http://www.oradev.com/ref_cursor.jsp)
2.  [http://download . Oracle . com/javase/6/docs/API/Java/SQL/callable statement . html](http://web.archive.org/web/20190224154700/http://download.oracle.com/javase/6/docs/api/java/sql/CallableStatement.html)
3.  [http://docsrv . SCO . com/JDK _ guide/JDBC/getstart/callable statement . doc . html](http://web.archive.org/web/20190224154700/http://docsrv.sco.com/JDK_guide/jdbc/getstart/callablestatement.doc.html)
4.  [http://on Java . com/pub/a/on Java/2003/08/13/stored _ procedures . html](http://web.archive.org/web/20190224154700/http://onjava.com/pub/a/onjava/2003/08/13/stored_procedures.html)









