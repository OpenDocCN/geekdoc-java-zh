# JDBC 报表示例-更新记录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-statement-example-update-a-record/>

下面的例子展示了如何通过 JDBC 语句更新表中的记录。要发出更新语句，调用`Statement.executeUpdate()`方法，如下所示:

```java
 Statement statement = dbConnection.createStatement();
// execute the update SQL stetement
statement.executeUpdate(updateTableSQL); 
```

完整示例…

```java
 package com.mkyong.jdbc;

import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCStatementUpdateExample {

	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
	private static final String DB_USER = "user";
	private static final String DB_PASSWORD = "password";

	public static void main(String[] argv) {

		try {

			updateRecordIntoDbUserTable();

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		}

	}

	private static void updateRecordIntoDbUserTable() throws SQLException {

		Connection dbConnection = null;
		Statement statement = null;

		String updateTableSQL = "UPDATE DBUSER"
				+ " SET USERNAME = 'mkyong_new' "
				+ " WHERE USER_ID = 1";

		try {
			dbConnection = getDBConnection();
			statement = dbConnection.createStatement();

			System.out.println(updateTableSQL);

			// execute update SQL stetement
			statement.execute(updateTableSQL);

			System.out.println("Record is updated to DBUSER table!");

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		} finally {

			if (statement != null) {
				statement.close();
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

用户名“用户标识= 1”被更新为新值“mkyong_new”。

```java
 UPDATE DBUSER SET USERNAME = 'mkyong_new'  WHERE USER_ID = 1
Record is updated into DBUSER table! 
```

[jdbc](http://web.archive.org/web/20190306165036/http://www.mkyong.com/tag/jdbc/) [statement](http://web.archive.org/web/20190306165036/http://www.mkyong.com/tag/statement/) [update](http://web.archive.org/web/20190306165036/http://www.mkyong.com/tag/update/)![](img/932444b07a2126c2b4d360420d2bc18f.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190306165036/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8236">







