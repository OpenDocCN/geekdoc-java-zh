# spring+mock ITO–无法模仿 save 方法？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-mockito-unable-to-mock-save-method/>

尝试模仿一个存储库`save()`方法，但是它总是返回 null？

*用 Spring Boot 2 + Spring 数据 JPA 测试的 PS*

```java
 @Test
    public void save_book_OK() throws Exception {

        Book newBook = new Book(1L, "Mockito Guide", "mkyong");
        when(mockRepository.save(newBook)).thenReturn(newBook);

		mockMvc.perform(post("/books")
			.content("{json}")
			.header(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON))
			.andExpect(status().isCreated());

    } 
```

## 解决办法

1.Mockito 使用`equals`进行参数匹配，尝试使用`ArgumentMatchers.any`进行`save`方法。

```java
 import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.*;

	@Test
    public void save_book_OK() throws Exception {

        Book newBook = new Book(1L, "Mockito Guide", "mkyong");
        when(mockRepository.save(any(Book.class))).thenReturn(newBook);

		//...

    } 
```

2.或者，为模型实现`equals`和`hashCode`。

```java
 package com.mkyong;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import java.math.BigDecimal;

@Entity
public class Book {

    @Id
    @GeneratedValue
    private Long id;
    private String name;
    private String author;

    //...

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Book book = (Book) o;

        if (id != null ? !id.equals(book.id) : book.id != null) return false;
        if (name != null ? !name.equals(book.name) : book.name != null) return false;
        return author != null ? author.equals(book.author) : book.author == null;
    }

    @Override
    public int hashCode() {
        int result = id != null ? id.hashCode() : 0;
        result = 31 * result + (name != null ? name.hashCode() : 0);
        result = 31 * result + (author != null ? author.hashCode() : 0);
        return result;
    }
} 
```

## 参考

*   [Mockito docs](http://web.archive.org/web/20201004113139/https://static.javadoc.io/org.mockito/mockito-core/2.24.5/org/mockito/Mockito.html)
*   [测试 Spring Boot 应用](http://web.archive.org/web/20201004113139/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications)

Tags : [mock](http://web.archive.org/web/20201004113139/https://mkyong.com/tag/mock/) [mockito](http://web.archive.org/web/20201004113139/https://mkyong.com/tag/mockito/) [spring boot](http://web.archive.org/web/20201004113139/https://mkyong.com/tag/spring-boot/) [spring data jpa](http://web.archive.org/web/20201004113139/https://mkyong.com/tag/spring-data-jpa/) [spring test](http://web.archive.org/web/20201004113139/https://mkyong.com/tag/spring-test/) [unit test](http://web.archive.org/web/20201004113139/https://mkyong.com/tag/unit-test/)<input type="hidden" id="mkyong-current-postId" value="14941">

### 相关文章

*   [Spring Boot +朱尼特 5 +莫奇托](/web/20201004113139/https://mkyong.com/spring-boot/spring-boot-junit-5-mockito/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link0)
*   [单元测试——什么是嘲讽？为什么呢？](/web/20201004113139/https://mkyong.com/unittest/unit-test-what-is-mocking-and-why/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link1)
*   [Mockito - when()需要一个必须](/web/20201004113139/https://mkyong.com/spring-boot/mockito-when-requires-an-argument-which-has-to-be-a-method-call-on-a-mock/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link2)的参数
*   [Spring Boot 测试无法自动连线 MockMvc](/web/20201004113139/https://mkyong.com/spring-boot/spring-boot-test-unable-to-autowired-mockmvc/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link3)
*   [Mockito -如何模拟存储库 findById thenRetu](/web/20201004113139/https://mkyong.com/spring-boot/mockito-how-to-mock-repository-findbyid-thenreturn-optional/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link4)

*   Spring Boot——如何初始化测试用的 Bean？
*   [弹簧支架集成测试示例](/web/20201004113139/https://mkyong.com/spring-boot/spring-rest-integration-test-example/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link6)
*   [Spring Boot 测试-如何禁用调试和信息 l](/web/20201004113139/https://mkyong.com/spring-boot/spring-boot-test-how-to-stop-debug-logs/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link7)
*   [JSONAssert -如何对 JSON 数据进行单元测试](/web/20201004113139/https://mkyong.com/java/jsonassert-how-to-unit-test-json-data/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link8)
*   [弹簧座+弹簧安全示例](/web/20201004113139/https://mkyong.com/spring-boot/spring-rest-spring-security-example/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link9)