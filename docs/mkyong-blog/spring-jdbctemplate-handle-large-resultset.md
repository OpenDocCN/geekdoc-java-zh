# Spring JdbcTemplate 处理大型结果集

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring/spring-jdbctemplate-handle-large-resultset/>

Spring `JdbcTemplate`示例获取一个大的`ResultSet`并处理它。

*PS 用 Java 8 和 Spring JDBC 5.1.4.RELEASE 测试过*

## 1.获取大的结果集

下面的 1.1 是一个从表中获取所有数据的经典`findAll`。

BookRepository.java

```java
 public List<Book> findAll() {
        return jdbcTemplate.query(
                "select * from books",
                (rs, rowNum) ->
                        new Book(
                                rs.getLong("id"),
                                rs.getString("name"),
                                rs.getBigDecimal("price")
                        )
        );

    } 
```

运行它，对于小数据，没问题。

```java
 List<Book> list = bookRepository.findAll();

	for (Book book : list) {
		//process it
	} 
```

如果表包含超过百万的数据，`findAll`方法中的`RowMapper`将忙于转换对象并将所有对象放入一个`List`，如果对象大小大于 Java 堆空间，请参见下面的错误:

```java
 java.lang.OutOfMemoryError: Java heap space 
```

## 2.解决办法

我们可以增加堆的大小，但是更好的解决方案是使用`RowCallbackHandler`逐行处理大的`ResultSet`。

```java
 import org.springframework.jdbc.core.RowCallbackHandler;

	jdbcTemplate.query("select * from books", new RowCallbackHandler() {
		public void processRow(ResultSet resultSet) throws SQLException {
			while (resultSet.next()) {
				String name = resultSet.getString("Name");
				// process it
			}
		}
	}); 
```

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20220316201920/https://github.com/mkyong/spring-boot.git)
$ cd spring-jdbc

## 参考

*   [RowCallbackHandler JavaDocs](http://web.archive.org/web/20220316201920/https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/jdbc/core/RowCallbackHandler.html)
*   [使用 Java 8 流 API 和 Spring 的 JdbcTemplate](http://web.archive.org/web/20220316201920/https://blog.apnic.net/2015/08/05/using-the-java-8-stream-api-with-springs-jdbctemplate/)
*   [春季 JDBC 文档](http://web.archive.org/web/20220316201920/https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc)
*   [Spring Boot JDBC 的例子](http://web.archive.org/web/20220316201920/https://www.mkyong.com/spring-boot/spring-boot-jdbc-examples/) _
*   [Java JDBC 教程](/web/20220316201920/https://mkyong.com/tutorials/jdbc-tutorials/)

<input type="hidden" id="mkyong-current-postId" value="15159">