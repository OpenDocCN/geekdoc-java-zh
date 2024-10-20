# java.sql.SQLException:无法识别服务器时区值“xx time”

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/java-sql-sqlexception-the-server-time-zone-value-xx-time-is-unrecognized/>

使用最新的`mysql-connector-java:8.0.16`与 MySQL 服务器建立 JDBC 连接，并遇到以下 SQLException:

*用*测试

*   MySQL 5.7
*   Java 8
*   JDBC 驱动程序，MySQL-连接器-java 8.0.16

```java
 try (Connection conn = DriverManager.getConnection(
			"jdbc:mysql://127.0.0.1:3306/test", "root", "password")) {

		//...

	} catch (Exception e) {
		e.printStackTrace();
	} 
```

输出

```java
 java.sql.SQLException: The server time zone value 'Malay Peninsula Standard Time' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:129)
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:97)
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:89)
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:63)
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:73)
	at com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:76)
	at com.mysql.cj.jdbc.ConnectionImpl.createNewIO(ConnectionImpl.java:835)
	at com.mysql.cj.jdbc.ConnectionImpl.<init>(ConnectionImpl.java:455)
	at com.mysql.cj.jdbc.ConnectionImpl.getInstance(ConnectionImpl.java:240)
	at com.mysql.cj.jdbc.NonRegisteringDriver.connect(NonRegisteringDriver.java:199)
	at java.sql/java.sql.DriverManager.getConnection(DriverManager.java:677)
	at java.sql/java.sql.DriverManager.getConnection(DriverManager.java:228)
	at com.mkyong.jdbc.JDBCExample.main(JDBCExample.java:23)
Caused by: com.mysql.cj.exceptions.InvalidConnectionAttributeException: The server time zone value 'Malay Peninsula Standard Time' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at java.base/jdk.internal.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:500)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:481)
	at com.mysql.cj.exceptions.ExceptionFactory.createException(ExceptionFactory.java:61)
	at com.mysql.cj.exceptions.ExceptionFactory.createException(ExceptionFactory.java:85)
	at com.mysql.cj.util.TimeUtil.getCanonicalTimezone(TimeUtil.java:132)
	at com.mysql.cj.protocol.a.NativeProtocol.configureTimezone(NativeProtocol.java:2243)
	at com.mysql.cj.protocol.a.NativeProtocol.initServerSession(NativeProtocol.java:2267)
	at com.mysql.cj.jdbc.ConnectionImpl.initializePropsFromServer(ConnectionImpl.java:1319)
	at com.mysql.cj.jdbc.ConnectionImpl.connectOneTryOnly(ConnectionImpl.java:966)
	at com.mysql.cj.jdbc.ConnectionImpl.createNewIO(ConnectionImpl.java:825)
	... 6 more 
```

## 1.解决办法

1.1 找到 MySQL 配置文件，例如`my.ini`或`my.cnf`。在文件末尾添加默认时区:

my.ini

```java
 # Indicates the InnoDB Cluster Port.
# innodbclusterport=3306

# Load mysql plugins at start."plugin_x ; plugin_y".
# plugin_load

# MySQL server's plugin configuration.
# loose_mysqlx_port=33060

# Set default time zone
default-time-zone = '+08:00' 
```

1.2 重启 MySQL 服务。

*附注:阅读这个 [MySQL 选项文件](http://web.archive.org/web/20210506192549/https://dev.mysql.com/doc/refman/5.7/en/option-files.html)，找出文件(`my.ini`或`my.cnf`)的位置。*

## 2.解决办法

或者，直接在 JDBC 连接字符串中传递一个`serverTimezone`属性。

```java
 try (Connection conn = DriverManager.getConnection(
			"jdbc:mysql://127.0.0.1:3306/test?serverTimezone=UTC", "root", "password")) {

		//...

	} catch (Exception e) {
		e.printStackTrace();
	} 
```

## 参考

*   [使用 JDBC 驱动程序连接 MySQL】](http://web.archive.org/web/20210506192549/https://www.mkyong.com/jdbc/how-to-connect-to-mysql-with-jdbc-driver-java/)
*   [MySQL 选项文件](http://web.archive.org/web/20210506192549/https://dev.mysql.com/doc/refman/5.7/en/option-files.html)
*   如何设置 MySQL 的时区？

Tags : [java](http://web.archive.org/web/20210506192549/https://mkyong.com/tag/java/) [jdbc](http://web.archive.org/web/20210506192549/https://mkyong.com/tag/jdbc/) [mysql](http://web.archive.org/web/20210506192549/https://mkyong.com/tag/mysql/)<input type="hidden" id="mkyong-current-postId" value="15138">