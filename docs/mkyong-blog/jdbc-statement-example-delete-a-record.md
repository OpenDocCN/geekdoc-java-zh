# JDBC 语句示例-删除记录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-statement-example-delete-a-record/>

下面的例子展示了如何通过 JDBC 语句从表中删除记录。要发出 delete 语句，调用`Statement.executeUpdate()`方法，如下所示:

```java
 Statement statement = dbConnection.createStatement();
// execute the delete SQL stetement
statement.executeUpdate(deleteTableSQL); 
```

完整示例…

```java
 package com.mkyong.jdbc;

import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCStatementDeleteExample {

	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
	private static final String DB_USER = "user";
	private static final String DB_PASSWORD = "password";

	public static void main(String[] argv) {

		try {

			deleteRecordFromDbUserTable();

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		}

	}

	private static void deleteRecordFromDbUserTable() throws SQLException {

		Connection dbConnection = null;
		Statement statement = null;

		String deleteTableSQL = "DELETE DBUSER WHERE USER_ID = 1";

		try {
			dbConnection = getDBConnection();
			statement = dbConnection.createStatement();

			System.out.println(deleteTableSQL);

			// execute delete SQL stetement
			statement.execute(deleteTableSQL);

			System.out.println("Record is deleted from DBUSER table!");

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

从表中删除“用户标识=1”的记录。

```java
 DELETE DBUSER WHERE USER_ID = 1
Record is deleted from DBUSER table! 
```

[delete](http://web.archive.org/web/20190224154912/http://www.mkyong.com/tag/delete/) [jdbc](http://web.archive.org/web/20190224154912/http://www.mkyong.com/tag/jdbc/) [statement](http://web.archive.org/web/20190224154912/http://www.mkyong.com/tag/statement/)![](img/4ef94fe99b9802155a5f5bf9ae311955.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224154912/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8240">







