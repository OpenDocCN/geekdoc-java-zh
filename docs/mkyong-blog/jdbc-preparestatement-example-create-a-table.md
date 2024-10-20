# JDBC 准备报表示例-创建表格

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-preparestatement-example-create-a-table/>

这里有一个例子来告诉你如何通过 JDBC PrepareStatement 在数据库中创建一个表。要发出 create 语句，调用`PrepareStatement.executeUpdate()`方法，如下所示:

```java
 PreparedStatement preparedStatement = dbConnection.prepareStatement(createTableSQL);
// execute create SQL stetement
preparedStatement.executeUpdate(); 
```

完整示例…

```java
 package com.mkyong.jdbc;

import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class JDBCPreparedStatementCreateExample {

	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
	private static final String DB_USER = "user";
	private static final String DB_PASSWORD = "password";

	public static void main(String[] argv) {

		try {

			createTable();

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		}

	}

	private static void createTable() throws SQLException {

		Connection dbConnection = null;
		PreparedStatement preparedStatement = null;

		String createTableSQL = "CREATE TABLE DBUSER1("
				+ "USER_ID NUMBER(5) NOT NULL, "
				+ "USERNAME VARCHAR(20) NOT NULL, "
				+ "CREATED_BY VARCHAR(20) NOT NULL, "
				+ "CREATED_DATE DATE NOT NULL, " + "PRIMARY KEY (USER_ID) "
				+ ")";

		try {
			dbConnection = getDBConnection();
			preparedStatement = dbConnection.prepareStatement(createTableSQL);

			System.out.println(createTableSQL);

			// execute create SQL stetement
			preparedStatement.executeUpdate();

			System.out.println("Table \"dbuser\" is created!");

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		} finally {

			if (preparedStatement != null) {
				preparedStatement.close();
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

## 结果

创建一个名为“DBUSER”的表。

```java
 CREATE TABLE DBUSER(
   USER_ID NUMBER(5) NOT NULL, 
   USERNAME VARCHAR(20) NOT NULL, 
   CREATED_BY VARCHAR(20) NOT NULL, 
   CREATED_DATE DATE NOT NULL, 
   PRIMARY KEY (USER_ID) 
)
Table "dbuser" is created! 
```

[create](http://web.archive.org/web/20190225101344/http://www.mkyong.com/tag/create/) [jdbc](http://web.archive.org/web/20190225101344/http://www.mkyong.com/tag/jdbc/) [preparedstatement](http://web.archive.org/web/20190225101344/http://www.mkyong.com/tag/preparedstatement/) [table](http://web.archive.org/web/20190225101344/http://www.mkyong.com/tag/table/)![](img/307f7db6733266aecbaaa95f8081dcce.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225101344/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8256">







