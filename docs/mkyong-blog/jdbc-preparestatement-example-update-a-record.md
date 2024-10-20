# JDBC 编制报表示例-更新记录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-preparestatement-example-update-a-record/>

这里有一个例子，向您展示如何通过 JDBC PreparedStatement 更新表中的记录。要发出更新语句，调用`PreparedStatement.executeUpdate()`方法，如下所示:

```java
 String updateTableSQL = "UPDATE DBUSER SET USERNAME = ? WHERE USER_ID = ?";
PreparedStatement preparedStatement = dbConnection.prepareStatement(updateTableSQL);
preparedStatement.setString(1, "mkyong_new_value");
preparedStatement.setInt(2, 1001);
// execute insert SQL stetement
preparedStatement .executeUpdate(); 
```

完整示例…

```java
 package com.mkyong.jdbc;

import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class JDBCPreparedStatementUpdateExample {

	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
	private static final String DB_USER = "user";
	private static final String DB_PASSWORD = "password";

	public static void main(String[] argv) {

		try {

			updateRecordToTable();

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		}

	}

	private static void updateRecordToTable() throws SQLException {

		Connection dbConnection = null;
		PreparedStatement preparedStatement = null;

		String updateTableSQL = "UPDATE DBUSER SET USERNAME = ? "
				                  + " WHERE USER_ID = ?";

		try {
			dbConnection = getDBConnection();
			preparedStatement = dbConnection.prepareStatement(updateTableSQL);

			preparedStatement.setString(1, "mkyong_new_value");
			preparedStatement.setInt(2, 1001);

			// execute update SQL stetement
			preparedStatement.executeUpdate();

			System.out.println("Record is updated to DBUSER table!");

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

用户名“用户标识= 1001”被更新为新值“mkyong_new_value”。

[jdbc](http://web.archive.org/web/20190223075356/http://www.mkyong.com/tag/jdbc/) [preparedstatement](http://web.archive.org/web/20190223075356/http://www.mkyong.com/tag/preparedstatement/) [update](http://web.archive.org/web/20190223075356/http://www.mkyong.com/tag/update/)![](img/2a56981a1f59ee274e8004a48c97195f.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223075356/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8269">







