# 单元测试——什么是嘲讽？为什么呢？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/unit-test-what-is-mocking-and-why/>

![mockito-logo](img/e659af74b9be8f511d87e5f06ca15ef9.png)

简单地说，模仿就是创建模仿真实对象行为的对象。请参考以下案例研究:

测试:

1.  Java 1.8
2.  JUnit 4.12
3.  Mockito 2.0.73-beta

**Mock Object**
Read this [Wikipedia Mock object](http://web.archive.org/web/20221024153706/https://en.wikipedia.org/wiki/Mock_object).

## 1.Java 示例

一个简单的作者和书籍的例子。

1.1 `BookService`按作者姓名返回图书列表。

BookService.java

```java
 package com.mkyong.examples.mock;

import java.util.List;

public interface BookService {

    List<Book> findBookByAuthor(String author);

} 
```

BookServiceImpl.java

```java
 package com.mkyong.examples.mock;

import java.util.List;

public class BookServiceImpl implements BookService {

    private BookDao bookDao;

    public BookDao getBookDao() {
        return bookDao;
    }

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    @Override
    public List<Book> findBookByAuthor(String name) {
        return bookDao.findBookByAuthor(name);
    }

} 
```

BookDao.java

```java
 package com.mkyong.examples.mock;

import java.util.List;

public interface BookDao {

    List<Book> findBookByAuthor(String author);

} 
```

BookDaoImpl.java

```java
 package com.mkyong.examples.mock;

import java.util.List;

public class BookDaoImpl implements BookDao {

    @Override
    public List<Book> findBookByAuthor(String name) {
		// init database
        // Connect to DB for data
        // return data
    }

} 
```

1.2 图书验证器。

BookValidatorService.java

```java
 package com.mkyong.examples.mock;

public interface BookValidatorService {

    boolean isValid(Book book);

} 
```

FakeBookValidatorService.java

```java
 package com.mkyong.examples.mock;

public class FakeBookValidatorService implements BookValidatorService {

    @Override
    public boolean isValid(Book book) {
        if (book == null)
            return false;

        if ("bot".equals(book.getName())) {
            return false;
        } else {
            return true;
        }

    }
} 
```

1.3 回顾了`AuthorServiceImpl`，它依赖于`BookService`(依赖于`BookDao`)和`BookValidatorService`，这使得单元测试有点难写。

AuthorService.java

```java
 package com.mkyong.examples.mock;

public interface AuthorService {

    int getTotalBooks(String author);

} 
```

AuthorServiceImpl.java

```java
 package com.mkyong.examples.mock;

import java.util.List;
import java.util.stream.Collectors;

public class AuthorServiceImpl implements AuthorService {

    private BookService bookService;
    private BookValidatorService bookValidatorService;

    public BookValidatorService getBookValidatorService() {
        return bookValidatorService;
    }

    public void setBookValidatorService(BookValidatorService bookValidatorService) {
        this.bookValidatorService = bookValidatorService;
    }

    public BookService getBookService() {
        return bookService;
    }

    public void setBookService(BookService bookService) {
        this.bookService = bookService;
    }

	//How to test this method ???
    @Override
    public int getTotalBooks(String author) {

        List<Book> books = bookService.findBookByAuthor(author);

        //filters some bot writers
        List<Book> filtered = books.stream().filter(
                x -> bookValidatorService.isValid(x))
                .collect(Collectors.toList());

        //other business logic

        return filtered.size();

    }
} 
```

## 2.单元测试

为`AuthorServiceImpl.getTotalBooks()`创建一个单元测试

2.1`AuthorServiceImpl`有两个依赖项，你需要确保两者都配置正确。

AuthorServiceTest.java

```java
 package com.mkyong.mock;

import com.mkyong.examples.mock.AuthorServiceImpl;
import com.mkyong.examples.mock.BookDaoImpl;
import com.mkyong.examples.mock.BookServiceImpl;
import com.mkyong.examples.mock.FakeBookValidatorService;
import org.junit.Test;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

public class AuthorServiceTest {

    @Test
    public void test_total_book_by_mock() {

		//1\. Setup
        AuthorServiceImpl obj = new AuthorServiceImpl();
        BookServiceImpl bookService = new BookServiceImpl();
        bookService.setBookDao(new BookDaoImpl()); //Where Dao connect to?
        obj.setBookService(bookService);
        obj.setBookValidatorService(new FakeBookValidatorService());

		//2\. Test method
        int qty = obj.getTotalBooks("mkyong");

		//3\. Verify result
        assertThat(qty, is(2));

    }

} 
```

要通过上面的单元测试，你需要在 DAO 层建立一个数据库，否则`bookService`将不返回任何东西。

2.3 执行上述测试的一些缺点:

1.  这个单元测试很慢，因为您需要启动一个数据库来从 DAO 获取数据。
2.  这个单元测试不是孤立的，它总是依赖于外部资源，比如数据库。
3.  这种单元测试不能确保测试条件总是相同的，数据库中的数据可能会随时间变化。
4.  测试一个简单的方法工作量太大，导致开发人员跳过测试。

2.4 **解决方案**
解决方案很明显，你需要一个`BookServiceImpl`类的修改版本——它将总是返回相同的数据进行测试，一个**模拟对象**！

**What is mocking?**
Again, mocking is creating objects that mimic the behavior of real objects.

## 3.单元测试–模拟对象

3.1 创建一个新的`MockBookServiceImpl`类，并总是为作者“mkyong”返回相同的图书集合。

MockBookServiceImpl.java

```java
 package com.mkyong.mock;

import com.mkyong.examples.mock.Book;
import com.mkyong.examples.mock.BookService;

import java.util.ArrayList;
import java.util.List;

//I am a mock object!
public class MockBookServiceImpl implements BookService {

    @Override
    public List<Book> findBookByAuthor(String author) {
        List<Book> books = new ArrayList<>();

        if ("mkyong".equals(author)) {
            books.add(new Book("mkyong in action"));
            books.add(new Book("abc in action"));
            books.add(new Book("bot"));
        }

        return books;
    }

    //implements other methods...

} 
```

3.2 再次更新单元测试。

AuthorServiceTest.java

```java
 package com.mkyong.mock;

import com.mkyong.examples.mock.AuthorServiceImpl;
import com.mkyong.examples.mock.FakeBookValidatorService;
import org.junit.Test;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

public class AuthorServiceTest {

    @Test
    public void test_total_book_by_mock() {

		//1\. Setup
        AuthorServiceImpl obj = new AuthorServiceImpl();

        /*BookServiceImpl bookService = new BookServiceImpl();
        bookService.setBookDao(new BookDaoImpl());
        obj.setBookService(bookService);*/

        obj.setBookService(new MockBookServiceImpl());
        obj.setBookValidatorService(new FakeBookValidatorService());

		//2\. Test method
        int qty = obj.getTotalBooks("mkyong");

		//3\. Verify result
        assertThat(qty, is(2));

    }

} 
```

上面的单元测试要好得多，快速，隔离(没有更多的数据库)并且测试条件(数据)总是相同的。

3.3 但是，像上面这样手动创建模拟对象有一些缺点:

1.  最后，您可以创建许多模拟对象(类)，只是为了单元测试的目的。
2.  如果接口包含许多方法，您需要覆盖它们中的每一个。
3.  还是工作量太大，而且乱七八糟！

3.4 **解决方案**
试试 [Mockito](http://web.archive.org/web/20221024153706/http://mockito.org/) ，一个简单而强大的嘲讽框架。

## 4.单元测试–mock ITO

4.1 再次更新单元测试，这一次，通过 Mockito 框架创建模拟对象。

pom.xml

```java
 <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>2.0.73-beta</version>
	</dependency> 
```

AuthorServiceTest.java

```java
 package com.mkyong.mock;

import com.mkyong.examples.mock.AuthorServiceImpl;
import com.mkyong.examples.mock.Book;
import com.mkyong.examples.mock.BookServiceImpl;
import com.mkyong.examples.mock.FakeBookValidatorService;
import org.junit.Test;

import java.util.Arrays;
import java.util.List;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;
import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.when;

public class AuthorServiceTest {

	    @Test
	    public void test_total_book_by_mockito() {

			//1\. Setup
	        List<Book> books = Arrays.asList(
	                new Book("mkyong in action"),
	                new Book("abc in action"),
	                new Book("bot"));

	        BookServiceImpl mockito = mock(BookServiceImpl.class);

	        //if the author is "mkyong", then return a 'books' object.
	        when(mockito.findBookByAuthor("mkyong")).thenReturn(books);

	        AuthorServiceImpl obj = new AuthorServiceImpl();
	        obj.setBookService(mockito);
	        obj.setBookValidatorService(new FakeBookValidatorService());

			//2\. Test method
	        int qty = obj.getTotalBooks("mkyong");

			//3\. Verify result
	        assertThat(qty, is(2));

	    }

} 
```

完成了。感谢您的反馈

## 参考

1.  [莫奇托官方网站](http://web.archive.org/web/20221024153706/http://mockito.org/)
2.  [DZone – Mockito](http://web.archive.org/web/20221024153706/https://dzone.com/refcardz/mockito)
3.  [维基百科–模拟对象](http://web.archive.org/web/20221024153706/https://en.wikipedia.org/wiki/Mock_object)
4.  [使用 Mockito 的单元测试](http://web.archive.org/web/20221024153706/http://www.vogella.com/tutorials/Mockito/article.html)

<input type="hidden" id="mkyong-current-postId" value="14010">