# 弹簧座错误处理示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-rest-error-handling-example/>

![](img/e58d8b7b707ed9aad8211cf66d069c3a.png)

在本文中，我们将向您展示 Spring Boot REST 应用程序中的错误处理。

使用的技术:

*   Spring Boot 2.1.2 .版本
*   弹簧 5.1.4 释放
*   maven3
*   Java 8

## 1./错误

1.1 默认情况下，Spring Boot 为处理所有错误的`/error`映射提供了一个`BasicErrorController`控制器，并为生成包含错误细节、HTTP 状态和异常消息的 JSON 响应提供了一个`getErrorAttributes`。

```java
 {	
	"timestamp":"2019-02-27T04:03:52.398+0000",
	"status":500,
	"error":"Internal Server Error",
	"message":"...",
	"path":"/path"
} 
```

BasicErrorController.java

```java
 package org.springframework.boot.autoconfigure.web.servlet.error;

//...

@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {

	//...

	@RequestMapping
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
		Map<String, Object> body = getErrorAttributes(request,
				isIncludeStackTrace(request, MediaType.ALL));
		HttpStatus status = getStatus(request);
		return new ResponseEntity<>(body, status);
	} 
```

在 IDE 中，在这个方法中放置一个断点，你将理解 Spring Boot 如何生成默认的 JSON 错误响应。

## 2.自定义异常

在 Spring Boot，我们可以使用`@ControllerAdvice`来处理自定义异常。

2.1 自定义异常。

BookNotFoundException.java

```java
 package com.mkyong.error;

public class BookNotFoundException extends RuntimeException {

    public BookNotFoundException(Long id) {
        super("Book id not found : " + id);
    }

} 
```

如果没有找到图书 id，控制器抛出上述`BookNotFoundException`

BookController.java

```java
 package com.mkyong;

//...

@RestController
public class BookController {

    @Autowired
    private BookRepository repository;

    // Find
    @GetMapping("/books/{id}")
    Book findOne(@PathVariable Long id) {
        return repository.findById(id)
                .orElseThrow(() -> new BookNotFoundException(id));
    }

	//...
} 
```

默认情况下，Spring Boot 生成以下 JSON 错误响应，http 500 error。

Terminal

```java
 curl localhost:8080/books/5

{
	"timestamp":"2019-02-27T04:03:52.398+0000",
	"status":500,
	"error":"Internal Server Error",
	"message":"Book id not found : 5",
	"path":"/books/5"
} 
```

2.2 如果没有找到图书 id，它应该返回 404 错误而不是 500，我们可以像这样覆盖状态代码:

CustomGlobalExceptionHandler.java

```java
 package com.mkyong.error;

import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

import javax.servlet.http.HttpServletResponse;
import javax.validation.ConstraintViolationException;
import java.io.IOException;
import java.util.Date;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

@ControllerAdvice
public class CustomGlobalExceptionHandler extends ResponseEntityExceptionHandler {

    // Let Spring BasicErrorController handle the exception, we just override the status code
    @ExceptionHandler(BookNotFoundException.class)
    public void springHandleNotFound(HttpServletResponse response) throws IOException {
        response.sendError(HttpStatus.NOT_FOUND.value());
    }

	//...
} 
```

2.3 它现在返回一个 404。

Terminal

```java
 curl localhost:8080/books/5

{
	"timestamp":"2019-02-27T04:21:17.740+0000",
	"status":404,
	"error":"Not Found",
	"message":"Book id not found : 5",
	"path":"/books/5"
} 
```

2.4 此外，我们可以定制整个 JSON 错误响应:

CustomErrorResponse.java

```java
 package com.mkyong.error;

import com.fasterxml.jackson.annotation.JsonFormat;

import java.time.LocalDateTime;

public class CustomErrorResponse {

    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd hh:mm:ss")
    private LocalDateTime timestamp;
    private int status;
    private String error;

    //...getters setters
} 
```

CustomGlobalExceptionHandler.java

```java
 package com.mkyong.error;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

import java.time.LocalDateTime;

@ControllerAdvice
public class CustomGlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(BookNotFoundException.class)
    public ResponseEntity<CustomErrorResponse> customHandleNotFound(Exception ex, WebRequest request) {

        CustomErrorResponse errors = new CustomErrorResponse();
        errors.setTimestamp(LocalDateTime.now());
        errors.setError(ex.getMessage());
        errors.setStatus(HttpStatus.NOT_FOUND.value());

        return new ResponseEntity<>(errors, HttpStatus.NOT_FOUND);

    }

	//...
} 
```

Terminal

```java
 curl localhost:8080/books/5
{
	"timestamp":"2019-02-27 12:40:45",
	"status":404,
	"error":"Book id not found : 5"
} 
```

## 3.JSR 303 验证错误

3.1 对于 Spring `@valid`验证错误，会抛出`handleMethodArgumentNotValid`

CustomGlobalExceptionHandler.java

```java
 package com.mkyong.error;

import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

import javax.servlet.http.HttpServletResponse;
import javax.validation.ConstraintViolationException;
import java.io.IOException;
import java.util.Date;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

@ControllerAdvice
public class CustomGlobalExceptionHandler extends ResponseEntityExceptionHandler {

    //...

    // @Validate For Validating Path Variables and Request Parameters
    @ExceptionHandler(ConstraintViolationException.class)
    public void constraintViolationException(HttpServletResponse response) throws IOException {
        response.sendError(HttpStatus.BAD_REQUEST.value());
    }

    // error handle for @Valid
    @Override
    protected ResponseEntity<Object>
    handleMethodArgumentNotValid(MethodArgumentNotValidException ex,
                                 HttpHeaders headers,
                                 HttpStatus status, WebRequest request) {

        Map<String, Object> body = new LinkedHashMap<>();
        body.put("timestamp", new Date());
        body.put("status", status.value());

        //Get all fields errors
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

## 4.ResponseEntityExceptionHandler

4.1 如果我们不确定 Spring Boot 抛出了什么异常，就在这个方法中放一个断点进行调试。

ResponseEntityExceptionHandler.java

```java
 package org.springframework.web.servlet.mvc.method.annotation;

//...
public abstract class ResponseEntityExceptionHandler {

	@ExceptionHandler({
			HttpRequestMethodNotSupportedException.class,
			HttpMediaTypeNotSupportedException.class,
			HttpMediaTypeNotAcceptableException.class,
			MissingPathVariableException.class,
			MissingServletRequestParameterException.class,
			ServletRequestBindingException.class,
			ConversionNotSupportedException.class,
			TypeMismatchException.class,
			HttpMessageNotReadableException.class,
			HttpMessageNotWritableException.class,
			MethodArgumentNotValidException.class,
			MissingServletRequestPartException.class,
			BindException.class,
			NoHandlerFoundException.class,
			AsyncRequestTimeoutException.class
		})
	@Nullable
	public final ResponseEntity<Object> handleException(Exception ex, WebRequest request) throws Exception {
		HttpHeaders headers = new HttpHeaders();

		if (ex instanceof HttpRequestMethodNotSupportedException) {
			HttpStatus status = HttpStatus.METHOD_NOT_ALLOWED;
			return handleHttpRequestMethodNotSupported((HttpRequestMethodNotSupportedException) ex, headers, status, request);
		}
		else if (ex instanceof HttpMediaTypeNotSupportedException) {
			HttpStatus status = HttpStatus.UNSUPPORTED_MEDIA_TYPE;
			return handleHttpMediaTypeNotSupported((HttpMediaTypeNotSupportedException) ex, headers, status, request);
		}
		//...
	}

	//...

} 
```

## 5.DefaultErrorAttributes

5.1 为了覆盖所有异常的默认 JSON 错误响应，创建一个 bean 并扩展`DefaultErrorAttributes`

CustomErrorAttributes.java

```java
 package com.mkyong.error;

import org.springframework.boot.web.servlet.error.DefaultErrorAttributes;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.WebRequest;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Map;

@Component
public class CustomErrorAttributes extends DefaultErrorAttributes {

    private static final DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");

    @Override
    public Map<String, Object> getErrorAttributes(WebRequest webRequest, boolean includeStackTrace) {

        // Let Spring handle the error first, we will modify later :)
        Map<String, Object> errorAttributes = super.getErrorAttributes(webRequest, includeStackTrace);

        // format & update timestamp
        Object timestamp = errorAttributes.get("timestamp");
        if (timestamp == null) {
            errorAttributes.put("timestamp", dateFormat.format(new Date()));
        } else {
            errorAttributes.put("timestamp", dateFormat.format((Date) timestamp));
        }

        // insert a new key
        errorAttributes.put("version", "1.2");

        return errorAttributes;

    }

} 
```

现在，日期时间被格式化，一个新字段–`version`被添加到 JSON 错误响应中。

```java
 curl localhost:8080/books/5

{
	"timestamp":"2019/02/27 13:34:24",
	"status":404,
	"error":"Not Found",
	"message":"Book id not found : 5",
	"path":"/books/5",
	"version":"1.2"
}

curl localhost:8080/abc

{
	"timestamp":"2019/02/27 13:35:10",
	"status":404,
	"error":"Not Found",
	"message":"No message available",
	"path":"/abc",
	"version":"1.2"
} 
```

完成了。

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20220820073043/https://github.com/mkyong/spring-boot.git)
$ cd spring-rest-error-handling
$ mvn spring-boot:run

## 参考

*   [Spring Boot 错误处理参考文献](http://web.archive.org/web/20220820073043/https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-error-handling)
*   [默认错误属性文档](http://web.archive.org/web/20220820073043/https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/web/servlet/error/DefaultErrorAttributes.html)
*   [ResponseEntityExceptionHandler](http://web.archive.org/web/20220820073043/https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/ResponseEntityExceptionHandler.html)
*   [弹簧座验证示例](http://web.archive.org/web/20220820073043/https://www.mkyong.com/spring-boot/spring-rest-validation-example/)
*   [Spring Boot 安全特征](http://web.archive.org/web/20220820073043/https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-security)
*   [Hello Spring Security with Boot](http://web.archive.org/web/20220820073043/https://docs.spring.io/spring-security/site/docs/current/guides/html5/helloworld-boot.html)
*   [维基百科–休息](http://web.archive.org/web/20220820073043/https://en.wikipedia.org/wiki/Representational_state_transfer)

<input type="hidden" id="mkyong-current-postId" value="14937">