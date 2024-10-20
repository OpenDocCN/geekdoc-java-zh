# 弹簧座验证示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-rest-validation-example/>

在本文中，我们将通过添加 bean 验证和自定义验证器来增强前面的 [Spring REST Hello World 示例](/web/20220904221328/https://mkyong.com/spring-boot/spring-rest-hello-world-example/)。

使用的技术:

*   Spring Boot 2.1.2 .版本
*   弹簧 5.1.4 释放
*   maven3
*   Java 8

## 1.控制器

再次查看之前的 REST 控制器:

BookController.java

```java
 package com.mkyong;

import com.mkyong.error.BookNotFoundException;
import com.mkyong.error.BookUnSupportedFieldPatchException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.util.StringUtils;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Map;

@RestController
public class BookController {

    @Autowired
    private BookRepository repository;

    // Find
    @GetMapping("/books")
    List<Book> findAll() {
        return repository.findAll();
    }

    // Save
    @PostMapping("/books")
	@ResponseStatus(HttpStatus.CREATED)
    Book newBook(@RequestBody Book newBook) {
        return repository.save(newBook);
    }

	 // Find
    @GetMapping("/books/{id}")
    Book findOne(@PathVariable Long id) {
        return repository.findById(id)
                .orElseThrow(() -> new BookNotFoundException(id));
    }

	//...
} 
```

## 2.Bean 验证(Hibernate 验证器)

2.1 如果任何 JSR-303 实现(如 Hibernate Validator)在类路径上可用，bean 验证将被自动启用。默认情况下，Spring Boot 会自动获取并下载 Hibernate 验证程序。

2.2 下面的 POST 请求将被通过，我们需要在`book`对象上实现 bean 验证，以确保像`name`、`author`和`price`这样的字段不为空。

```java
 @PostMapping("/books")
    Book newBook(@RequestBody Book newBook) {
        return repository.save(newBook);
    } 
```

```java
 curl -v -X POST localhost:8080/books -H "Content-type:application/json" -d "{\"name\":\"ABC\"}" 
```

2.3 用`[javax.validation.constraints.*](http://web.archive.org/web/20220904221328/https://docs.jboss.org/hibernate/beanvalidation/spec/2.0/api/javax/validation/constraints/package-summary.html)`注释对 bean 进行注释。

Book.java

```java
 package com.mkyong;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.validation.constraints.DecimalMin;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.NotNull;
import java.math.BigDecimal;

@Entity
public class Book {

    @Id
    @GeneratedValue
    private Long id;

    @NotEmpty(message = "Please provide a name")
    private String name;

    @NotEmpty(message = "Please provide a author")
    private String author;

    @NotNull(message = "Please provide a price")
    @DecimalMin("1.00")
    private BigDecimal price;

    //...
} 
```

2.4 在`@RequestBody`中增加`@Valid`。完成，现在启用 bean 验证。

BookController.java

```java
 import javax.validation.Valid;

@RestController
public class BookController {

    @PostMapping("/books")
    Book newBook(@Valid @RequestBody Book newBook) {
        return repository.save(newBook);
    }
	//...
} 
```

2.5 尝试再次向 REST 端点发送 POST 请求。如果 bean 验证失败，将触发一个`MethodArgumentNotValidException`。默认情况下，Spring 会发回一个 HTTP 状态 **400 错误请求**，但是没有错误细节。

```java
 curl -v -X POST localhost:8080/books -H "Content-type:application/json" -d "{\"name\":\"ABC\"}"

Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> POST /books HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.55.1
> Accept: */*
> Content-type:application/json
> Content-Length: 32
>
* upload completely sent off: 32 out of 32 bytes
< HTTP/1.1 400
< Content-Length: 0
< Date: Wed, 20 Feb 2019 13:02:30 GMT
< Connection: close
< 
```

2.6 上面的错误响应不友好，我们可以捕捉`MethodArgumentNotValidException`并像这样覆盖响应:

CustomGlobalExceptionHandler.java

```java
 package com.mkyong.error;

import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

import java.util.Date;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

@ControllerAdvice
public class CustomGlobalExceptionHandler extends ResponseEntityExceptionHandler {

    // error handle for @Valid
    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex,
                                                                  HttpHeaders headers,
                                                                  HttpStatus status, WebRequest request) {

        Map<String, Object> body = new LinkedHashMap<>();
        body.put("timestamp", new Date());
        body.put("status", status.value());

        //Get all errors
        List<String> errors = ex.getBindingResult()
                .getFieldErrors()
                .stream()
                .map(x -> x.getDefaultMessage())
                .collect(Collectors.toList());

        body.put("errors", errors);

        return new ResponseEntity<>(body, headers, status);

    }

} 
```

2.7 再试一次。完成了。

```java
 curl -v -X POST localhost:8080/books -H "Content-type:application/json" -d "{\"name\":\"ABC\"}"

{
	"timestamp":"2019-02-20T13:21:27.653+0000",
	"status":400,
	"errors":[
		"Please provide a author",
		"Please provide a price"
	]
} 
```

## 3.路径变量验证

3.1 我们也可以直接在路径变量甚至请求参数上应用`javax.validation.constraints.*`注释。

3.2 在类级别应用`@Validated`，并在路径变量上添加`javax.validation.constraints.*`注释，如下所示:

BookController.java

```java
 import org.springframework.validation.annotation.Validated;
import javax.validation.constraints.Min;

@RestController
@Validated // class level
public class BookController {

    @GetMapping("/books/{id}")
    Book findOne(@PathVariable @Min(1) Long id) { //jsr 303 annotations
        return repository.findById(id)
                .orElseThrow(() -> new BookNotFoundException(id));
    }

	//...
} 
```

3.3 默认的错误信息是好的，只是错误代码 500 不合适。

```java
 curl -v localhost:8080/books/0

{
	"timestamp":"2019-02-20T13:27:43.638+0000",
	"status":500,
	"error":"Internal Server Error",
	"message":"findOne.id: must be greater than or equal to 1",
	"path":"/books/0"
} 
```

3.4 如果`@Validated`失败，将触发一个`ConstraintViolationException`，我们可以这样覆盖错误代码:

CustomGlobalExceptionHandler.java

```java
 package com.mkyong.error;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

import javax.servlet.http.HttpServletResponse;
import javax.validation.ConstraintViolationException;
import java.io.IOException;

@ControllerAdvice
public class CustomGlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(ConstraintViolationException.class)
    public void constraintViolationException(HttpServletResponse response) throws IOException {
        response.sendError(HttpStatus.BAD_REQUEST.value());
    }

	//..
} 
```

```java
 curl -v localhost:8080/books/0

{
	"timestamp":"2019-02-20T13:35:59.808+0000",
	"status":400,
	"error":"Bad Request",
	"message":"findOne.id: must be greater than or equal to 1",
	"path":"/books/0"
} 
```

## 4.自定义验证程序

4.1 我们将为`author`字段创建一个自定义验证器，只允许 4 个作者保存到数据库中。

Author.java

```java
 package com.mkyong.error.validator;

import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

@Target({FIELD})
@Retention(RUNTIME)
@Constraint(validatedBy = AuthorValidator.class)
@Documented
public @interface Author {

    String message() default "Author is not allowed.";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};

} 
```

AuthorValidator.java

```java
 package com.mkyong.error.validator;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;
import java.util.Arrays;
import java.util.List;

public class AuthorValidator implements ConstraintValidator<Author, String> {

    List<String> authors = Arrays.asList("Santideva", "Marie Kondo", "Martin Fowler", "mkyong");

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {

        return authors.contains(value);

    }
} 
```

Book.java

```java
 package com.mkyong;

import com.mkyong.error.validator.Author;

import javax.persistence.Entity;
import javax.validation.constraints.NotEmpty;
//...

@Entity
public class Book {

    @Author
    @NotEmpty(message = "Please provide a author")
    private String author;

	//... 
```

4.2 测试一下。如果自定义验证器失败，它将触发一个`MethodArgumentNotValidException`

```java
 curl -v -X POST localhost:8080/books 
	-H "Content-type:application/json" 
	-d "{\"name\":\"Spring REST tutorials\", \"author\":\"abc\",\"price\":\"9.99\"}"

{
	"timestamp":"2019-02-20T13:49:59.971+0000",
	"status":400,
	"errors":["Author is not allowed."]
} 
```

## 5.弹簧综合测试

5.1 用`MockMvc`进行测试

BookControllerTest.java

```java
 package com.mkyong;

import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.hamcrest.Matchers.*;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.times;
import static org.mockito.Mockito.verify;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
@ActiveProfiles("test")
public class BookControllerTest {

    private static final ObjectMapper om = new ObjectMapper();

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private BookRepository mockRepository;

    /*
        {
            "timestamp":"2019-03-05T09:34:13.280+0000",
            "status":400,
            "errors":["Author is not allowed.","Please provide a price","Please provide a author"]
        }
     */
    @Test
    public void save_emptyAuthor_emptyPrice_400() throws Exception {

        String bookInJson = "{\"name\":\"ABC\"}";

        mockMvc.perform(post("/books")
                .content(bookInJson)
                .header(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON))
                .andDo(print())
                .andExpect(status().isBadRequest())
                .andExpect(jsonPath("$.timestamp", is(notNullValue())))
                .andExpect(jsonPath("$.status", is(400)))
                .andExpect(jsonPath("$.errors").isArray())
                .andExpect(jsonPath("$.errors", hasSize(3)))
                .andExpect(jsonPath("$.errors", hasItem("Author is not allowed.")))
                .andExpect(jsonPath("$.errors", hasItem("Please provide a author")))
                .andExpect(jsonPath("$.errors", hasItem("Please provide a price")));

        verify(mockRepository, times(0)).save(any(Book.class));

    }

    /*
        {
            "timestamp":"2019-03-05T09:34:13.207+0000",
            "status":400,
            "errors":["Author is not allowed."]
        }
     */
    @Test
    public void save_invalidAuthor_400() throws Exception {

        String bookInJson = "{\"name\":\" Spring REST tutorials\", \"author\":\"abc\",\"price\":\"9.99\"}";

        mockMvc.perform(post("/books")
                .content(bookInJson)
                .header(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON))
                .andDo(print())
                .andExpect(status().isBadRequest())
                .andExpect(jsonPath("$.timestamp", is(notNullValue())))
                .andExpect(jsonPath("$.status", is(400)))
                .andExpect(jsonPath("$.errors").isArray())
                .andExpect(jsonPath("$.errors", hasSize(1)))
                .andExpect(jsonPath("$.errors", hasItem("Author is not allowed.")));

        verify(mockRepository, times(0)).save(any(Book.class));

    }

} 
```

5.2 用`TestRestTemplate`进行测试

BookControllerRestTemplateTest.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.json.JSONException;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.skyscreamer.jsonassert.JSONAssert;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.*;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.junit4.SpringRunner;

import static org.junit.Assert.assertEquals;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.times;
import static org.mockito.Mockito.verify;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT) // for restTemplate
@ActiveProfiles("test")
public class BookControllerRestTemplateTest {

    private static final ObjectMapper om = new ObjectMapper();

    @Autowired
    private TestRestTemplate restTemplate;

    @MockBean
    private BookRepository mockRepository;

    /*
        {
            "timestamp":"2019-03-05T09:34:13.280+0000",
            "status":400,
            "errors":["Author is not allowed.","Please provide a price","Please provide a author"]
        }
     */
    @Test
    public void save_emptyAuthor_emptyPrice_400() throws JSONException {

        String bookInJson = "{\"name\":\"ABC\"}";

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        HttpEntity<String> entity = new HttpEntity<>(bookInJson, headers);

        // send json with POST
        ResponseEntity<String> response = restTemplate.postForEntity("/books", entity, String.class);
        //printJSON(response);

        String expectedJson = "{\"status\":400,\"errors\":[\"Author is not allowed.\",\"Please provide a price\",\"Please provide a author\"]}";
        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        JSONAssert.assertEquals(expectedJson, response.getBody(), false);

        verify(mockRepository, times(0)).save(any(Book.class));

    }

    /*
        {
            "timestamp":"2019-03-05T09:34:13.207+0000",
            "status":400,
            "errors":["Author is not allowed."]
        }
     */
    @Test
    public void save_invalidAuthor_400() throws JSONException {

        String bookInJson = "{\"name\":\" Spring REST tutorials\", \"author\":\"abc\",\"price\":\"9.99\"}";

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        HttpEntity<String> entity = new HttpEntity<>(bookInJson, headers);

        //Try exchange
        ResponseEntity<String> response = restTemplate.exchange("/books", HttpMethod.POST, entity, String.class);

        String expectedJson = "{\"status\":400,\"errors\":[\"Author is not allowed.\"]}";
        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        JSONAssert.assertEquals(expectedJson, response.getBody(), false);

        verify(mockRepository, times(0)).save(any(Book.class));

    }

    private static void printJSON(Object object) {
        String result;
        try {
            result = om.writerWithDefaultPrettyPrinter().writeValueAsString(object);
            System.out.println(result);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
    }

} 
```

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20220904221328/https://github.com/mkyong/spring-boot.git)
$ cd spring-rest-validation
$ mvn spring-boot:run

## 参考

*   [javax . validation . constraints](http://web.archive.org/web/20220904221328/https://docs.jboss.org/hibernate/beanvalidation/spec/2.0/api/javax/validation/constraints/package-summary.html)
*   [Spring Boot 验证功能](http://web.archive.org/web/20220904221328/https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-validation)
*   [Spring Boot +朱尼特 5 +莫奇托](http://web.archive.org/web/20220904221328/https://www.mkyong.com/spring-boot/spring-boot-junit-5-mockito/)
*   [春歇你好天下例](http://web.archive.org/web/20220904221328/https://www.mkyong.com/spring-boot/spring-rest-hello-world-example/)
*   [cURL–发布请求示例](http://web.archive.org/web/20220904221328/https://www.mkyong.com/spring/curl-post-request-examples/)
*   [维基百科–休息](http://web.archive.org/web/20220904221328/https://en.wikipedia.org/wiki/Representational_state_transfer)

<input type="hidden" id="mkyong-current-postId" value="14928">