# Spring Boot·JDBC 存储过程示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-jdbc-stored-procedure-examples/>

![spring jdbc SimpleJdbcCall](img/2a4c3b472663535e5e27a2beb54a1cdb.png)

在本教程中，我们将向您展示如何使用 Spring Boot·JDBC`SimpleJdbcCall`从 Oracle 数据库中调用存储过程和存储函数。

使用的技术:

*   Spring Boot 2.1.2 .版本
*   春季 JDBC 5.1.4 .发布
*   Oracle 数据库 19c
*   HikariCP 3.2.0
*   maven3
*   Java 8

与`JdbcTemplate`不同，Spring Boot 没有自动创建任何`SimpleJdbcCall`，我们必须手动创建它。

**Note**
This example extends the previous [Spring Boot JDBC examples](/web/20230101144912/https://mkyong.com/spring-boot/spring-boot-jdbc-examples/), adds support for `SimpleJdbcCall`

## 1.测试数据

1.1 创建一个表，保存 4 本书进行测试。

```java
 CREATE TABLE BOOKS(
    ID NUMBER GENERATED ALWAYS as IDENTITY(START with 1 INCREMENT by 1),
    NAME VARCHAR2(100) NOT NULL,
    PRICE NUMBER(15, 2) NOT NULL,
    CONSTRAINT book_pk PRIMARY KEY (ID)
); 
```

```java
 List<Book> books = Arrays.asList(
			new Book("Thinking in Java", new BigDecimal("46.32")),
			new Book("Mkyong in Java", new BigDecimal("1.99")),
			new Book("Getting Clojure", new BigDecimal("37.3")),
			new Book("Head First Android Development", new BigDecimal("41.19"))
	);

	books.forEach(book -> {
		log.info("Saving...{}", book.getName());
		bookRepository.save(book);
	}); 
```

## 2.存储过程

2.1 返回单个结果的存储过程。

```java
 CREATE OR REPLACE PROCEDURE get_book_by_id(
        p_id IN BOOKS.ID%TYPE,
        o_name OUT BOOKS.NAME%TYPE,
        o_price OUT BOOKS.PRICE%TYPE)
    AS
    BEGIN

        SELECT NAME , PRICE INTO o_name, o_price from BOOKS WHERE ID = p_id;

    END; 
```

2.2 我们可以通过`@PostConstruct`初始化`SimpleJdbcCall`。

StoredProcedure1.java

```java
 package com.mkyong.sp;

import com.mkyong.Book;
import com.mkyong.repository.BookRepository;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcCall;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import java.math.BigDecimal;
import java.util.Map;
import java.util.Optional;

@Component
public class StoredProcedure1 {

    private static final Logger log = LoggerFactory.getLogger(StoredProcedure1.class);

    @Autowired
    @Qualifier("jdbcBookRepository")
    private BookRepository bookRepository;

    @Autowired
    private JdbcTemplate jdbcTemplate;

    private SimpleJdbcCall simpleJdbcCall;

    // init SimpleJdbcCall
    @PostConstruct
    void init() {
        // o_name and O_NAME, same
        jdbcTemplate.setResultsMapCaseInsensitive(true);

        simpleJdbcCall = new SimpleJdbcCall(jdbcTemplate)
                .withProcedureName("get_book_by_id");

    }

    private static final String SQL_STORED_PROC = ""
            + " CREATE OR REPLACE PROCEDURE get_book_by_id "
            + " ("
            + "  p_id IN BOOKS.ID%TYPE,"
            + "  o_name OUT BOOKS.NAME%TYPE,"
            + "  o_price OUT BOOKS.PRICE%TYPE"
            + " ) AS"
            + " BEGIN"
            + "  SELECT NAME, PRICE INTO o_name, o_price from BOOKS WHERE ID = p_id;"
            + " END;";

    public void start() {

        log.info("Creating Store Procedures and Function...");
        jdbcTemplate.execute(SQL_STORED_PROC);

        /* Test Stored Procedure */
        Book book = findById(2L).orElseThrow(IllegalArgumentException::new);

        // Book{id=2, name='Mkyong in Java', price=1.99}
        System.out.println(book);

    }

    Optional<Book> findById(Long id) {

        SqlParameterSource in = new MapSqlParameterSource()
                .addValue("p_id", id);

        Optional result = Optional.empty();

        try {

            Map out = simpleJdbcCall.execute(in);

            if (out != null) {
                Book book = new Book();
                book.setId(id);
                book.setName((String) out.get("O_NAME"));
                book.setPrice((BigDecimal) out.get("O_PRICE"));
                result = Optional.of(book);
            }

        } catch (Exception e) {
            // ORA-01403: no data found, or any java.sql.SQLException
            System.err.println(e.getMessage());
        }

        return result;
    }

} 
```

## 3.存储过程#SYS_REFCURSOR

3.1 返回引用游标的存储过程。

```java
 CREATE OR REPLACE PROCEDURE get_book_by_name(
   p_name IN BOOKS.NAME%TYPE,
   o_c_book OUT SYS_REFCURSOR)
AS
BEGIN

  OPEN o_c_book FOR
  SELECT * FROM BOOKS WHERE NAME LIKE '%' || p_name || '%';

END; 
```

3.2 `BeanPropertyRowMapper`将光标结果映射到`book`对象。

StoredProcedure2.java

```java
 package com.mkyong.sp;

import com.mkyong.Book;
import com.mkyong.repository.BookRepository;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcCall;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import java.util.Collections;
import java.util.List;
import java.util.Map;

@Component
public class StoredProcedure2 {

    private static final Logger log = LoggerFactory.getLogger(StoredProcedure2.class);

    @Autowired
    @Qualifier("jdbcBookRepository")
    private BookRepository bookRepository;

    @Autowired
    private JdbcTemplate jdbcTemplate;

    private SimpleJdbcCall simpleJdbcCallRefCursor;

    // init SimpleJdbcCall
    @PostConstruct
    public void init() {
        // o_name and O_NAME, same
        jdbcTemplate.setResultsMapCaseInsensitive(true);

        // Convert o_c_book SYS_REFCURSOR to List<Book>
        simpleJdbcCallRefCursor = new SimpleJdbcCall(jdbcTemplate)
                .withProcedureName("get_book_by_name")
                .returningResultSet("o_c_book",
                        BeanPropertyRowMapper.newInstance(Book.class));

    }

    private static final String SQL_STORED_PROC_REF = ""
            + " CREATE OR REPLACE PROCEDURE get_book_by_name "
            + " ("
            + "  p_name IN BOOKS.NAME%TYPE,"
            + "  o_c_book OUT SYS_REFCURSOR"
            + " ) AS"
            + " BEGIN"
            + "  OPEN o_c_book FOR"
            + "  SELECT * FROM BOOKS WHERE NAME LIKE '%' || p_name || '%';"
            + " END;";

    public void start() {

		log.info("Creating Store Procedures and Function...");
        jdbcTemplate.execute(SQL_STORED_PROC_REF);

        /* Test Stored Procedure RefCursor */
        List<Book> books = findBookByName("Java");

        // Book{id=1, name='Thinking in Java', price=46.32}
        // Book{id=2, name='Mkyong in Java', price=1.99}
        books.forEach(x -> System.out.println(x));

    }

    List<Book> findBookByName(String name) {

        SqlParameterSource paramaters = new MapSqlParameterSource()
                .addValue("p_name", name);

        Map out = simpleJdbcCallRefCursor.execute(paramaters);

        if (out == null) {
            return Collections.emptyList();
        } else {
            return (List) out.get("o_c_book");
        }

    }

} 
```

## 4.存储功能

4.1 为测试创建两个函数。

```java
 CREATE OR REPLACE FUNCTION get_price_by_id(p_id IN BOOKS.ID%TYPE)
RETURN NUMBER
IS o_price BOOKS.PRICE%TYPE;
BEGIN
    SELECT PRICE INTO o_price from BOOKS WHERE ID = p_id;
    RETURN(o_price);
END;

CREATE OR REPLACE FUNCTION get_database_time
RETURN VARCHAR2
IS o_date VARCHAR2(20);
BEGIN
    SELECT TO_CHAR(SYSDATE, 'DD-MON-YYYY HH:MI:SS') INTO o_date FROM dual;
    RETURN(o_date);
END; 
```

4.2.对于存储函数，用`SimpleJdbcCall.executeFunction`调用它

StoredFunction.java

```java
 package com.mkyong.sp;

import com.mkyong.repository.BookRepository;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcCall;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import java.math.BigDecimal;

@Component
public class StoredFunction {

    private static final Logger log = LoggerFactory.getLogger(StoredFunction.class);

    @Autowired
    @Qualifier("jdbcBookRepository")
    private BookRepository bookRepository;

    @Autowired
    private JdbcTemplate jdbcTemplate;

    private SimpleJdbcCall simpleJdbcCallFunction1;
    private SimpleJdbcCall simpleJdbcCallFunction2;

    // init SimpleJdbcCall
    @PostConstruct
    public void init() {

        jdbcTemplate.setResultsMapCaseInsensitive(true);

        simpleJdbcCallFunction1 = new SimpleJdbcCall(jdbcTemplate)
			.withFunctionName("get_price_by_id");

        simpleJdbcCallFunction2 = new SimpleJdbcCall(jdbcTemplate)
			.withFunctionName("get_database_time");
    }

    private static final String SQL_STORED_FUNCTION_1 = ""
            + " CREATE OR REPLACE FUNCTION get_price_by_id(p_id IN BOOKS.ID%TYPE) "
            + " RETURN NUMBER"
            + " IS o_price BOOKS.PRICE%TYPE;"
            + " BEGIN"
            + "  SELECT PRICE INTO o_price from BOOKS WHERE ID = p_id;"
            + "  RETURN(o_price);"
            + " END;";

    private static final String SQL_STORED_FUNCTION_2 = ""
            + " CREATE OR REPLACE FUNCTION get_database_time "
            + " RETURN VARCHAR2"
            + " IS o_date VARCHAR2(20);"
            + " BEGIN"
            + "  SELECT TO_CHAR(SYSDATE, 'DD-MON-YYYY HH:MI:SS') INTO o_date FROM dual;"
            + "  RETURN(o_date);"
            + " END;";

    public void start() {

        log.info("Creating Store Procedures and Function...");
        jdbcTemplate.execute(SQL_STORED_FUNCTION_1);
        jdbcTemplate.execute(SQL_STORED_FUNCTION_2);

        /* Test Stored Function 1 */
        SqlParameterSource in = new MapSqlParameterSource()
                .addValue("p_id", 3L);
        BigDecimal price = simpleJdbcCallFunction1.executeFunction(BigDecimal.class, in);
        System.out.println(price);  // 37.3

        /* Test Stored Function 2 */
        String database_time = simpleJdbcCallFunction2.executeFunction(String.class);
        System.out.println(database_time); // e.g current date, 23-JUL-2019 05:08:44

    }

} 
```

总而言之:

*   对于存储过程，`SimpleJdbcCall.execute`。
*   对于存储功能，`SimpleJdbcCall.executeFunction`

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20230101144912/https://github.com/mkyong/spring-boot.git)
$ cd spring-jdbc/sp

## 参考

*   [Oracle 创建函数](http://web.archive.org/web/20230101144912/https://docs.oracle.com/cd/B19306_01/server.102/b14200/statements_5009.htm)
*   [Oracle 存储函数示例](http://web.archive.org/web/20230101144912/https://www.mkyong.com/oracle/oracle-plsql-create-function-example/)
*   [Oracle 开发和使用存储过程](http://web.archive.org/web/20230101144912/https://docs.oracle.com/cd/B28359_01/appdev.111/b28843/tdddg_procedures.htm)
*   [JDBC 可调用语句-存储过程输出参数示例](http://web.archive.org/web/20230101144912/https://www.mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-out-parameter-example/)
*   [春季 JDBC 文档](http://web.archive.org/web/20230101144912/https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc)
*   [使用 SQL 数据库](http://web.archive.org/web/20230101144912/https://docs.spring.io/spring-boot/docs/2.1.3.RELEASE/reference/htmlsingle/#boot-features-sql)
*   [Java JDBC 教程](/web/20230101144912/https://mkyong.com/tutorials/jdbc-tutorials/)
*   [使用引用游标返回记录集](http://web.archive.org/web/20230101144912/https://oracle-base.com/articles/misc/using-ref-cursors-to-return-recordsets)

<input type="hidden" id="mkyong-current-postId" value="15151">