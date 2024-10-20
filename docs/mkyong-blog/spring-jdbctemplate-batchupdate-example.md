# Spring JdbcTemplate batchUpdate()示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring/spring-jdbctemplate-batchupdate-example/>

Spring `JdbcTemplate`批量插入、批量更新以及`@Transactional`示例。

使用的技术:

*   Spring Boot 2.1.2 .版本
*   春季 JDBC 5.1.4 .发布
*   maven3
*   Java 8

## 1.批量插入

1.1 一起插入一批 SQL 插入。

BookRepository.java

```java
 import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.BatchPreparedStatementSetter;

    public int[] batchInsert(List<Book> books) {

        return this.jdbcTemplate.batchUpdate(
			"insert into books (name, price) values(?,?)",
			new BatchPreparedStatementSetter() {

				public void setValues(PreparedStatement ps, int i) throws SQLException {
					ps.setString(1, books.get(i).getName());
					ps.setBigDecimal(2, books.get(i).getPrice());
				}

				public int getBatchSize() {
					return books.size();
				}

			});
    } 
```

1.2 如果批量太大，我们可以按较小的批量拆分。

BookRepository.java

```java
 import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.ParameterizedPreparedStatementSetter;

    public int[][] batchInsert(List<Book> books, int batchSize) {

        int[][] updateCounts = jdbcTemplate.batchUpdate(
                "insert into books (name, price) values(?,?)",
                books,
                batchSize,
                new ParameterizedPreparedStatementSetter<Book>() {
                    public void setValues(PreparedStatement ps, Book argument) 
						throws SQLException {
                        ps.setString(1, argument.getName());
                        ps.setBigDecimal(2, argument.getPrice());
                    }
                });
        return updateCounts;

    } 
```

## 2.批量更新

2.1 与 SQL update 语句相同。

BookRepository.java

```java
 import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.BatchPreparedStatementSetter;

	public int[] batchUpdate(List<Book> books) {

        return this.jdbcTemplate.batchUpdate(
                "update books set price = ? where id = ?",
                new BatchPreparedStatementSetter() {

                    public void setValues(PreparedStatement ps, int i) 
						throws SQLException {
                        ps.setBigDecimal(1, books.get(i).getPrice());
                        ps.setLong(2, books.get(i).getId());
                    }

                    public int getBatchSize() {
                        return books.size();
                    }

                });

    } 
```

2.2 按批次大小更新。

BookRepository.java

```java
 import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.ParameterizedPreparedStatementSetter;

	public int[][] batchUpdate(List<Book> books, int batchSize) {

        int[][] updateCounts = jdbcTemplate.batchUpdate(
                "update books set price = ? where id = ?",
                books,
                batchSize,
                new ParameterizedPreparedStatementSetter<Book>() {
                    public void setValues(PreparedStatement ps, Book argument) 
						throws SQLException {
                        ps.setBigDecimal(1, argument.getPrice());
                        ps.setLong(2, argument.getId());
                    }
                });
        return updateCounts;

    } 
```

## 3.奔跑

3.1 创建一个表来测试批量插入和更新。

SpringBootApplication.java

```java
 startBookBatchUpdateApp(1000);

void startBookBatchUpdateApp(int size) {

	jdbcTemplate.execute("CREATE TABLE books(" +
			"id SERIAL, name VARCHAR(255), price NUMERIC(15, 2))");

	List<Book> books = new ArrayList();
	for (int count = 0; count < size; count++) {
		books.add(new Book(NameGenerator.randomName(20), new BigDecimal(1.99)));
	}

	// batch insert
	bookRepository.batchInsert(books);

	List<Book> bookFromDatabase = bookRepository.findAll();

	// count
	log.info("Total books: {}", bookFromDatabase.size());
	// random
	log.info("{}", bookRepository.findById(2L).orElseThrow(IllegalArgumentException::new));
	log.info("{}", bookRepository.findById(500L).orElseThrow(IllegalArgumentException::new));

	// update all books to 9.99
	bookFromDatabase.forEach(x -> x.setPrice(new BigDecimal(9.99)));

	// batch update
	bookRepository.batchUpdate(bookFromDatabase);

	List<Book> updatedList = bookRepository.findAll();

	// count
	log.info("Total books: {}", updatedList.size());
	// random
	log.info("{}", bookRepository.findById(2L).orElseThrow(IllegalArgumentException::new));
	log.info("{}", bookRepository.findById(500L).orElseThrow(IllegalArgumentException::new));

} 
```

输出

```java
 Total books: 1000
Book{id=2, name='FcRzgpauFtwfWibpzWog', price=1.99}
Book{id=500, name='htDvtGmksjfGmXGKOCaR', price=1.99}

Total books: 1000
Book{id=2, name='FcRzgpauFtwfWibpzWog', price=9.99}
Book{id=500, name='htDvtGmksjfGmXGKOCaR', price=9.99} 
```

## 4.@事务性

4.1 使用`@Transactional`，任何故障都会导致整个操作回滚，不会添加任何书籍。

BookRepository.java

```java
 @Transactional
    public int[][] batchInsert(List<Book> books, int batchSize) {

        int[][] updateCounts = jdbcTemplate.batchUpdate(
                "insert into books (name, price) values(?,?)",
                books,
                batchSize,
                new ParameterizedPreparedStatementSetter<Book>() {
                    public void setValues(PreparedStatement ps, Book argument) throws SQLException {
                        ps.setString(1, argument.getName());
                        ps.setBigDecimal(2, argument.getPrice());
                    }
                });
        return updateCounts;

    } 
```

4.2 尝试批量插入 1000 本书，其中#500 包含一个错误，并且整批将被回滚，没有书将被插入。

SpringBootApplication.java

```java
 startBookBatchUpdateRollBack(1000);

void startBookBatchUpdateRollBack(int size) {

	jdbcTemplate.execute("CREATE TABLE books(" +
			"id SERIAL, name VARCHAR(255), price NUMERIC(15, 2))");

	List<Book> books = new ArrayList();
	for (int count = 0; count < size; count++) {
		if (count == 500) {
			// Create an invalid data for id 500, test rollback
			// Name max 255, this book has length of 300
			books.add(new Book(NameGenerator.randomName(300), new BigDecimal(1.99)));
			continue;
		}
		books.add(new Book(NameGenerator.randomName(20), new BigDecimal(1.99)));
	}

	try {
		// with @Transactional, any error, entire batch will be rolled back
		bookRepository.batchInsert(books, 100);
	} catch (Exception e) {
		System.err.println(e.getMessage());
	}

	List<Book> bookFromDatabase = bookRepository.findAll();

	// count = 0 , id 500 error, roll back all
	log.info("Total books: {}", bookFromDatabase.size());

} 
```

输出

```java
 PreparedStatementCallback; SQL [insert into books (name, price) values(?,?)]; Value too long for column "NAME VARCHAR(255)" 

Total books: 0 
```

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20221229115239/https://github.com/mkyong/spring-boot.git)
$ cd spring-jdbc

## 参考

*   [JDBC 批处理操作 JavaDocs](http://web.archive.org/web/20221229115239/https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc-advanced-jdbc)
*   [春季 JDBC 文档](http://web.archive.org/web/20221229115239/https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc)
*   [Spring Boot JDBC 的例子](http://web.archive.org/web/20221229115239/https://www.mkyong.com/spring-boot/spring-boot-jdbc-examples/) _
*   [Java JDBC 教程](http://web.archive.org/web/20221229115239/https://www.mkyong.com/tutorials/jdbc-tutorials/)

<input type="hidden" id="mkyong-current-postId" value="3781">