# java.sql.SQLException:不允许操作:序号绑定和命名绑定不能组合！

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/java-sql-sqlexception-operation-not-allowed-ordinal-binding-and-named-binding-cannot-be-combined/>

顺序绑定或索引绑定:

```java
 String name = stat.getString(2);
	BigDecimal salary = stat.getBigDecimal(3);
	Timestamp createdDate = stat.getTimestamp(4); 
```

命名绑定:

```java
 String name = stat.getString("NAME");
	BigDecimal salary = stat.getBigDecimal("SALARY");
	Timestamp createdDate = stat.getTimestamp("CREATED_DATE"); 
```

如果我们像这样混合两者:

```java
 String name = stat.getString(2);
	BigDecimal salary = stat.getBigDecimal("SALARY");
	Timestamp createdDate = stat.getTimestamp(4); 
```

错误:

```java
 java.sql.SQLException: operation not allowed: 
		Ordinal binding and Named binding cannot be combined! 
```

## JDBC 可赎回声明

调用进出存储过程的`CallableStatement`示例:

```java
 // IN with ordinal binding
	callableStatement.setInt(1, 999);

	// OUT with name binding
	String name = callableStatement.getString("NAME");
    BigDecimal salary = callableStatement.getBigDecimal("SALARY");
    Timestamp createdDate = callableStatement.getTimestamp("CREATED_DATE"); 
```

输出

```java
 java.sql.SQLException: operation not allowed: 
		Ordinal binding and Named binding cannot be combined! 
```

要修复此问题，请将 all 更新为序号绑定或名称绑定:

```java
 callableStatement.setInt(1, 999);

	String name = callableStatement.getString(2);
	BigDecimal salary = callableStatement.getBigDecimal(3);
	Timestamp createdDate = callableStatement.getTimestamp(4); 
```

## 参考

*   [使用存储过程](http://web.archive.org/web/20230101144948/https://docs.oracle.com/javase/tutorial/jdbc/basics/storedprocedures.html)
*   [参数示例中的 JDBC 可调用语句-存储过程](http://web.archive.org/web/20230101144948/http://www.mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-in-parameter-example/)
*   [JDBC 可调用语句-存储过程输出参数示例](http://web.archive.org/web/20230101144948/http://www.mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-out-parameter-example/)

<input type="hidden" id="mkyong-current-postId" value="15130">