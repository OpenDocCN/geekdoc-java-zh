# JDBC 编制报表示例-删除记录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-preparestatement-example-delete-a-record/>

这里有一个例子，向您展示如何通过 JDBC PreparedStatement 从表中删除记录。要发出 delete 语句，调用`PreparedStatement.executeUpdate()`方法，如下所示:

```java
 String deleteSQL = "DELETE DBUSER WHERE USER_ID = ?";
PreparedStatement preparedStatement = dbConnection.prepareStatement(deleteSQL);
preparedStatement.setInt(1, 1001);
// execute delete SQL stetement
preparedStatement.executeUpdate(); 
```

完整示例…

```java
 package com.mkyong.jdbc;

import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class JDBCPreparedStatementSelectExample {

	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
	private static final String DB_USER = "user";
	private static final String DB_PASSWORD = "password";

	public static void main(String[] argv) {

		try {

			deleteRecordFromTable();

		} catch (SQLException e) {

			System.out.println(e.getMessage());

		}

	}

	private static void deleteRecordFromTable() throws SQLException {

		Connection dbConnection = null;
		PreparedStatement preparedStatement = null;

		String deleteSQL = "DELETE DBUSER WHERE USER_ID = ?";

		try {
			dbConnection = getDBConnection();
			preparedStatement = dbConnection.prepareStatement(deleteSQL);
			preparedStatement.setInt(1, 1001);

			// execute delete SQL stetement
			preparedStatement.executeUpdate();

			System.out.println("Record is deleted!");

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

从表中删除“用户 id=1001”的记录。

[delete](http://web.archive.org/web/20190223065002/http://www.mkyong.com/tag/delete/) [jdbc](http://web.archive.org/web/20190223065002/http://www.mkyong.com/tag/jdbc/) [preparestatement](http://web.archive.org/web/20190223065002/http://www.mkyong.com/tag/preparestatement/)![](img/e17fa80f077f9c3c99d14e93a12ec476.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223065002/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8280">







