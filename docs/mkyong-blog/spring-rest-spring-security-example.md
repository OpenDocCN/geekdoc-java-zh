# 弹簧座+弹簧安全示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-rest-spring-security-example/>

![logo](img/e5a5f014c9beee9b3290c675ff2163fe.png)

在本文中，我们将增强前面的 [Spring REST 验证示例](/web/20221117050521/https://mkyong.com/spring-boot/spring-rest-validation-example/)，通过添加 Spring Security 为请求的 URL(REST API 端点)执行认证和授权

使用的技术:

*   Spring Boot 2.1.2 .版本
*   弹簧 5.1.4 释放
*   Spring Security 5.1.3 .发布
*   春季数据 JPA 2.1.4.RELEASE
*   H2 内存数据库 1.4.197
*   Tomcat Embed 9.0.14
*   JUnit 4.12
*   maven3
*   Java 8

## 1.项目目录

![project directory](img/6aa3d8b2457605ed5f013d56585308ec.png)

## 2.专家

包括弹簧安全的`spring-boot-starter-security`和弹簧安全集成测试的`spring-security-test`。

pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <artifactId>spring-rest-security</artifactId>
    <packaging>jar</packaging>
    <name>Spring Boot REST API Example</name>
    <version>1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.2.RELEASE</version>
    </parent>

    <!-- Java 8 -->
    <properties>
        <java.version>1.8</java.version>
        <downloadSources>true</downloadSources>
        <downloadJavadocs>true</downloadJavadocs>
    </properties>

    <dependencies>

        <!-- spring mvc, rest -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- spring security -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <!-- spring security test -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- jpa, crud repository -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- in-memory database  -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
        </dependency>

        <!-- unit test rest -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- test patch operation need this -->
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.7</version>
            <scope>test</scope>
        </dependency>

        <!-- hot swapping, disable cache for template, enable live reload -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>

    </dependencies>

    <build>
        <plugins>

            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <addResources>true</addResources>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.0</version>
            </plugin>

        </plugins>

    </build>
</project> 
```

项目依赖关系:

```java
 > mvn dependency:tree

[INFO] --- maven-dependency-plugin:3.1.1:tree (default-cli) @ spring-rest-security ---
[INFO] org.springframework.boot:spring-rest-security:jar:1.0
[INFO] +- org.springframework.boot:spring-boot-starter-web:jar:2.1.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:2.1.2.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-starter-logging:jar:2.1.2.RELEASE:compile
[INFO] |  |  |  +- ch.qos.logback:logback-classic:jar:1.2.3:compile
[INFO] |  |  |  |  \- ch.qos.logback:logback-core:jar:1.2.3:compile
[INFO] |  |  |  +- org.apache.logging.log4j:log4j-to-slf4j:jar:2.11.1:compile
[INFO] |  |  |  |  \- org.apache.logging.log4j:log4j-api:jar:2.11.1:compile
[INFO] |  |  |  \- org.slf4j:jul-to-slf4j:jar:1.7.25:compile
[INFO] |  |  +- javax.annotation:javax.annotation-api:jar:1.3.2:compile
[INFO] |  |  \- org.yaml:snakeyaml:jar:1.23:runtime
[INFO] |  +- org.springframework.boot:spring-boot-starter-json:jar:2.1.2.RELEASE:compile
[INFO] |  |  +- com.fasterxml.jackson.core:jackson-databind:jar:2.9.8:compile
[INFO] |  |  |  +- com.fasterxml.jackson.core:jackson-annotations:jar:2.9.0:compile
[INFO] |  |  |  \- com.fasterxml.jackson.core:jackson-core:jar:2.9.8:compile
[INFO] |  |  +- com.fasterxml.jackson.datatype:jackson-datatype-jdk8:jar:2.9.8:compile
[INFO] |  |  +- com.fasterxml.jackson.datatype:jackson-datatype-jsr310:jar:2.9.8:compile
[INFO] |  |  \- com.fasterxml.jackson.module:jackson-module-parameter-names:jar:2.9.8:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-tomcat:jar:2.1.2.RELEASE:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:9.0.14:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:9.0.14:compile
[INFO] |  |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:9.0.14:compile
[INFO] |  +- org.hibernate.validator:hibernate-validator:jar:6.0.14.Final:compile
[INFO] |  |  +- javax.validation:validation-api:jar:2.0.1.Final:compile
[INFO] |  |  +- org.jboss.logging:jboss-logging:jar:3.3.2.Final:compile
[INFO] |  |  \- com.fasterxml:classmate:jar:1.4.0:compile
[INFO] |  +- org.springframework:spring-web:jar:5.1.4.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-beans:jar:5.1.4.RELEASE:compile
[INFO] |  \- org.springframework:spring-webmvc:jar:5.1.4.RELEASE:compile
[INFO] |     +- org.springframework:spring-context:jar:5.1.4.RELEASE:compile
[INFO] |     \- org.springframework:spring-expression:jar:5.1.4.RELEASE:compile
[INFO] +- org.springframework.boot:spring-boot-starter-security:jar:2.1.2.RELEASE:compile
[INFO] |  +- org.springframework:spring-aop:jar:5.1.4.RELEASE:compile
[INFO] |  +- org.springframework.security:spring-security-config:jar:5.1.3.RELEASE:compile
[INFO] |  \- org.springframework.security:spring-security-web:jar:5.1.3.RELEASE:compile
[INFO] +- org.springframework.security:spring-security-test:jar:5.1.3.RELEASE:test
[INFO] |  +- org.springframework.security:spring-security-core:jar:5.1.3.RELEASE:compile
[INFO] |  +- org.springframework:spring-core:jar:5.1.4.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-jcl:jar:5.1.4.RELEASE:compile
[INFO] |  \- org.springframework:spring-test:jar:5.1.4.RELEASE:test
[INFO] +- org.springframework.boot:spring-boot-starter-data-jpa:jar:2.1.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-aop:jar:2.1.2.RELEASE:compile
[INFO] |  |  \- org.aspectj:aspectjweaver:jar:1.9.2:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-jdbc:jar:2.1.2.RELEASE:compile
[INFO] |  |  +- com.zaxxer:HikariCP:jar:3.2.0:compile
[INFO] |  |  \- org.springframework:spring-jdbc:jar:5.1.4.RELEASE:compile
[INFO] |  +- javax.transaction:javax.transaction-api:jar:1.3:compile
[INFO] |  +- javax.xml.bind:jaxb-api:jar:2.3.1:compile
[INFO] |  |  \- javax.activation:javax.activation-api:jar:1.2.0:compile
[INFO] |  +- org.hibernate:hibernate-core:jar:5.3.7.Final:compile
[INFO] |  |  +- javax.persistence:javax.persistence-api:jar:2.2:compile
[INFO] |  |  +- org.javassist:javassist:jar:3.23.1-GA:compile
[INFO] |  |  +- net.bytebuddy:byte-buddy:jar:1.9.7:compile
[INFO] |  |  +- antlr:antlr:jar:2.7.7:compile
[INFO] |  |  +- org.jboss:jandex:jar:2.0.5.Final:compile
[INFO] |  |  +- org.dom4j:dom4j:jar:2.1.1:compile
[INFO] |  |  \- org.hibernate.common:hibernate-commons-annotations:jar:5.0.4.Final:compile
[INFO] |  +- org.springframework.data:spring-data-jpa:jar:2.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework.data:spring-data-commons:jar:2.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-orm:jar:5.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-tx:jar:5.1.4.RELEASE:compile
[INFO] |  |  \- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO] |  \- org.springframework:spring-aspects:jar:5.1.4.RELEASE:compile
[INFO] +- com.h2database:h2:jar:1.4.197:compile
[INFO] +- org.springframework.boot:spring-boot-starter-test:jar:2.1.2.RELEASE:test
[INFO] |  +- org.springframework.boot:spring-boot-test:jar:2.1.2.RELEASE:test
[INFO] |  +- org.springframework.boot:spring-boot-test-autoconfigure:jar:2.1.2.RELEASE:test
[INFO] |  +- com.jayway.jsonpath:json-path:jar:2.4.0:test
[INFO] |  |  \- net.minidev:json-smart:jar:2.3:test
[INFO] |  |     \- net.minidev:accessors-smart:jar:1.2:test
[INFO] |  |        \- org.ow2.asm:asm:jar:5.0.4:test
[INFO] |  +- junit:junit:jar:4.12:test
[INFO] |  +- org.assertj:assertj-core:jar:3.11.1:test
[INFO] |  +- org.mockito:mockito-core:jar:2.23.4:test
[INFO] |  |  +- net.bytebuddy:byte-buddy-agent:jar:1.9.7:test
[INFO] |  |  \- org.objenesis:objenesis:jar:2.6:test
[INFO] |  +- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] |  +- org.hamcrest:hamcrest-library:jar:1.3:test
[INFO] |  +- org.skyscreamer:jsonassert:jar:1.5.0:test
[INFO] |  |  \- com.vaadin.external.google:android-json:jar:0.0.20131108.vaadin1:test
[INFO] |  \- org.xmlunit:xmlunit-core:jar:2.6.2:test
[INFO] +- org.apache.httpcomponents:httpclient:jar:4.5.7:test
[INFO] |  +- org.apache.httpcomponents:httpcore:jar:4.4.10:test
[INFO] |  \- commons-codec:commons-codec:jar:1.11:test
[INFO] \- org.springframework.boot:spring-boot-devtools:jar:2.1.2.RELEASE:compile (optional)
[INFO]    +- org.springframework.boot:spring-boot:jar:2.1.2.RELEASE:compile
[INFO]    \- org.springframework.boot:spring-boot-autoconfigure:jar:2.1.2.RELEASE:compile 
```

## 3.弹簧控制器

再次回顾一下 Book 控制器，稍后我们将集成 Spring Security 来保护 REST 端点。

BookController.java

```java
 package com.mkyong;

import com.mkyong.error.BookNotFoundException;
import com.mkyong.error.BookUnSupportedFieldPatchException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.util.StringUtils;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;
import javax.validation.constraints.Min;
import java.util.List;
import java.util.Map;

@RestController
@Validated
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
    Book newBook(@Valid @RequestBody Book newBook) {
        return repository.save(newBook);
    }

    // Find
    @GetMapping("/books/{id}")
    Book findOne(@PathVariable @Min(1) Long id) {
        return repository.findById(id)
                .orElseThrow(() -> new BookNotFoundException(id));
    }

    // Save or update
    @PutMapping("/books/{id}")
    Book saveOrUpdate(@RequestBody Book newBook, @PathVariable Long id) {

        return repository.findById(id)
                .map(x -> {
                    x.setName(newBook.getName());
                    x.setAuthor(newBook.getAuthor());
                    x.setPrice(newBook.getPrice());
                    return repository.save(x);
                })
                .orElseGet(() -> {
                    newBook.setId(id);
                    return repository.save(newBook);
                });
    }

    // update author only
    @PatchMapping("/books/{id}")
    Book patch(@RequestBody Map<String, String> update, @PathVariable Long id) {

        return repository.findById(id)
                .map(x -> {

                    String author = update.get("author");
                    if (!StringUtils.isEmpty(author)) {
                        x.setAuthor(author);

                        // better create a custom method to update a value = :newValue where id = :id
                        return repository.save(x);
                    } else {
                        throw new BookUnSupportedFieldPatchException(update.keySet());
                    }

                })
                .orElseGet(() -> {
                    throw new BookNotFoundException(id);
                });

    }

    @DeleteMapping("/books/{id}")
    void deleteBook(@PathVariable Long id) {
        repository.deleteById(id);
    }

} 
```

*P.S 此处未列出其他组件或库，请参考前面的[弹簧座验证示例](/web/20221117050521/https://mkyong.com/spring-boot/spring-rest-validation-example/)*

## 4.春天安全

4.1 创建一个新的`@Configuration`类并扩展`WebSecurityConfigurerAdapter`。在下面的例子中，我们将使用 [HTTP 基本认证](http://web.archive.org/web/20221117050521/https://en.wikipedia.org/wiki/Basic_access_authentication)来保护其余的端点。阅读注释，了解自我解释。

SpringSecurityConfig.java

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {

    // Create 2 users for demo
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        auth.inMemoryAuthentication()
                .withUser("user").password("{noop}password").roles("USER")
                .and()
                .withUser("admin").password("{noop}password").roles("USER", "ADMIN");

    }

    // Secure the endpoins with HTTP Basic authentication
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http
                //HTTP Basic authentication
                .httpBasic()
                .and()
                .authorizeRequests()
                .antMatchers(HttpMethod.GET, "/books/**").hasRole("USER")
                .antMatchers(HttpMethod.POST, "/books").hasRole("ADMIN")
                .antMatchers(HttpMethod.PUT, "/books/**").hasRole("ADMIN")
                .antMatchers(HttpMethod.PATCH, "/books/**").hasRole("ADMIN")
                .antMatchers(HttpMethod.DELETE, "/books/**").hasRole("ADMIN")
                .and()
                .csrf().disable()
                .formLogin().disable();
    }

    /*@Bean
    public UserDetailsService userDetailsService() {
        //ok for demo
        User.UserBuilder users = User.withDefaultPasswordEncoder();

        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(users.username("user").password("password").roles("USER").build());
        manager.createUser(users.username("admin").password("password").roles("USER", "ADMIN").build());
        return manager;
    }*/

} 
```

4.2 完成后，上述 Spring REST API 端点受到 Spring Security 的保护🙂

阅读更多信息:

*   [Spring Boot 安全特征](http://web.archive.org/web/20221117050521/https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-security)
*   [春季安全参考](http://web.archive.org/web/20221117050521/https://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/)

## 5.Spring Boot

正常的 Spring Boot 应用程序启动其余的端点，并插入 3 本书到 H2 数据库进行演示。

StartBookApplication

```java
 package com.mkyong;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import java.math.BigDecimal;

@SpringBootApplication
public class StartBookApplication {

    // start everything
    public static void main(String[] args) {
        SpringApplication.run(StartBookApplication.class, args);
    }

    @Bean
    CommandLineRunner initDatabase(BookRepository repository) {
        return args -> {
            repository.save(new Book("A Guide to the Bodhisattva Way of Life", "Santideva", new BigDecimal("15.41")));
            repository.save(new Book("The Life-Changing Magic of Tidying Up", "Marie Kondo", new BigDecimal("9.69")));
            repository.save(new Book("Refactoring: Improving the Design of Existing Code", "Martin Fowler", new BigDecimal("47.99")));
        };
    }
} 
```

## 6.演示

6.1 启动 Spring Boot 应用程序。

```java
 mvn spring-boot:run 
```

6.2 一个正常的`GET`和`POST`会返回一个`401`，所有端点都被保护，需要认证。

```java
 > curl localhost:8080/books

{	
	"timestamp":"2019-02-25T04:05:14.709+0000",
	"status":401,
	"error":"Unauthorized",
	"message":"Unauthorized",
	"path":"/books"
}

> curl -X POST localhost:8080/books -H "Content-type:application/json" 
	-d {\"name\":\"ABC\",\"author\":\"mkyong\",\"price\":\"8.88\"}

{
	"timestamp":"2019-02-25T04:11:17.150+0000",
	"status":401,
	"error":"Unauthorized",
	"message":"Unauthorized",
	"path":"/books"
} 
```

6.3 发送一个`GET`请求和`user`登录。

```java
 > curl localhost:8080/books -u user:password
[
	{"id":1,"name":"A Guide to the Bodhisattva Way of Life","author":"Santideva","price":15.41},
	{"id":2,"name":"The Life-Changing Magic of Tidying Up","author":"Marie Kondo","price":9.69},
	{"id":3,"name":"Refactoring: Improving the Design of Existing Code","author":"Martin Fowler","price":47.99}
]

> curl localhost:8080/books/1 -u admin:password

{
	"id":1,
	"name":"A Guide to the Bodhisattva Way of Life",
	"author":"Santideva",
	"price":15.41
} 
```

6.4 尝试用“用户”登录发送一个`POST`请求，它将返回`403, Forbidden`错误。这是因为`user`无权发送`POST`请求。

```java
 > curl -X POST localhost:8080/books -H "Content-type:application/json" 
	-d {\"name\":\"ABC\",\"author\":\"mkyong\",\"price\":\"8.88\"} -u user:password

{
	"timestamp":"2019-02-25T04:16:58.702+0000",
	"status":403,
	"error":"Forbidden",
	"message":"Forbidden",
	"path":"/books"
} 
```

再次检查 Spring 安全配置。要发送`POST,PUT,PATCH or DELETE`请求，我们需要`admin`

SpringSecurityConfig.java

```java
 @Override
    protected void configure(HttpSecurity http) throws Exception {

        http
                //HTTP Basic authentication
                .httpBasic()
                .and()
                .authorizeRequests()
                .antMatchers(HttpMethod.GET, "/books/**").hasRole("USER")
                .antMatchers(HttpMethod.POST, "/books").hasRole("ADMIN")
                .antMatchers(HttpMethod.PUT, "/books/**").hasRole("ADMIN")
                .antMatchers(HttpMethod.PATCH, "/books/**").hasRole("ADMIN")
                .antMatchers(HttpMethod.DELETE, "/books/**").hasRole("ADMIN")
                .and()
                .csrf().disable()
                .formLogin().disable();
    }

} 
```

6.5 尝试使用`admin`登录发送`POST`请求

```java
 > curl -X POST localhost:8080/books -H "Content-type:application/json" 
	-d {\"name\":\"ABC\",\"author\":\"mkyong\",\"price\":\"8.88\"} -u admin:password

{
	"id":4,
	"name":"ABC",
	"author":"mkyong",
	"price":8.88
}

> curl localhost:8080/books -u user:password

[
	{"id":1,"name":"A Guide to the Bodhisattva Way of Life","author":"Santideva","price":15.41},
	{"id":2,"name":"The Life-Changing Magic of Tidying Up","author":"Marie Kondo","price":9.69},
	{"id":3,"name":"Refactoring: Improving the Design of Existing Code","author":"Martin Fowler","price":47.99},
	{"id":4,"name":"ABC","author":"mkyong","price":8.88}
] 
```

## 7.春季安全集成测试

7.1 用`@WithMockUser`和`MockMvc`进行测试

BookControllerTest.java

```java
 package com.mkyong;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import java.math.BigDecimal;
import java.util.Optional;

import static org.hamcrest.Matchers.is;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
@ActiveProfiles("test")
public class BookControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private BookRepository mockRepository;

    @Before
    public void init() {
        Book book = new Book(1L, "A Guide to the Bodhisattva Way of Life", "Santideva", new BigDecimal("15.41"));
        when(mockRepository.findById(1L)).thenReturn(Optional.of(book));
    }

    //@WithMockUser(username = "USER")
    @WithMockUser("USER")
    @Test
    public void find_login_ok() throws Exception {

        mockMvc.perform(get("/books/1"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.id", is(1)))
                .andExpect(jsonPath("$.name", is("A Guide to the Bodhisattva Way of Life")))
                .andExpect(jsonPath("$.author", is("Santideva")))
                .andExpect(jsonPath("$.price", is(15.41)));
    }

    @Test
    public void find_nologin_401() throws Exception {
        mockMvc.perform(get("/books/1"))
                .andDo(print())
                .andExpect(status().isUnauthorized());
    }

} 
```

7.2 用`TestRestTemplate`进行测试

BookControllerRestTemplateTest.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.skyscreamer.jsonassert.JSONAssert;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.junit4.SpringRunner;

import java.math.BigDecimal;
import java.util.Optional;

import static org.junit.Assert.assertEquals;
import static org.mockito.Mockito.when;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@ActiveProfiles("test")
public class BookControllerRestTemplateTest {

    private static final ObjectMapper om = new ObjectMapper();

    //@WithMockUser is not working with TestRestTemplate
    @Autowired
    private TestRestTemplate restTemplate;

    @MockBean
    private BookRepository mockRepository;

    @Before
    public void init() {
        Book book = new Book(1L, "A Guide to the Bodhisattva Way of Life", "Santideva", new BigDecimal("15.41"));
        when(mockRepository.findById(1L)).thenReturn(Optional.of(book));
    }

    @Test
    public void find_login_ok() throws Exception {

        String expected = "{id:1,name:\"A Guide to the Bodhisattva Way of Life\",author:\"Santideva\",price:15.41}";

        ResponseEntity<String> response = restTemplate
                .withBasicAuth("user", "password")
                .getForEntity("/books/1", String.class);

        printJSON(response);

        assertEquals(MediaType.APPLICATION_JSON_UTF8, response.getHeaders().getContentType());
        assertEquals(HttpStatus.OK, response.getStatusCode());

        JSONAssert.assertEquals(expected, response.getBody(), false);

    }

    @Test
    public void find_nologin_401() throws Exception {

        String expected = "{\"status\":401,\"error\":\"Unauthorized\",\"message\":\"Unauthorized\",\"path\":\"/books/1\"}";

        ResponseEntity<String> response = restTemplate
                .getForEntity("/books/1", String.class);

        printJSON(response);

        assertEquals(MediaType.APPLICATION_JSON_UTF8, response.getHeaders().getContentType());
        assertEquals(HttpStatus.UNAUTHORIZED, response.getStatusCode());

        JSONAssert.assertEquals(expected, response.getBody(), false);

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

*附注:`@WithMockUser`不支持`TestRestTemplate`，我们需要使用` . withBasicAuth`* 验证用户

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20221117050521/https://github.com/mkyong/spring-boot.git)
$ cd spring-rest-security
$ mvn spring-boot:run

## 参考

*   [春季安全参考](http://web.archive.org/web/20221117050521/https://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/)
*   [Spring Boot 安全特征](http://web.archive.org/web/20221117050521/https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-security)
*   [Hello Spring Security with Boot](http://web.archive.org/web/20221117050521/https://docs.spring.io/spring-security/site/docs/current/guides/html5/helloworld-boot.html)
*   [Spring 数据安全示例](http://web.archive.org/web/20221117050521/https://github.com/spring-projects/spring-data-examples/tree/master/rest/security)
*   [Spring Boot +朱尼特 5 +莫奇托](http://web.archive.org/web/20221117050521/https://www.mkyong.com/spring-boot/spring-boot-junit-5-mockito/)
*   [春歇你好天下例](http://web.archive.org/web/20221117050521/https://www.mkyong.com/spring-boot/spring-rest-hello-world-example/)
*   [cURL–发布请求示例](http://web.archive.org/web/20221117050521/https://www.mkyong.com/spring/curl-post-request-examples/)
*   [维基百科–休息](http://web.archive.org/web/20221117050521/https://en.wikipedia.org/wiki/Representational_state_transfer)
*   [HTTP 基本认证](http://web.archive.org/web/20221117050521/https://en.wikipedia.org/wiki/Basic_access_authentication)
*   [StudentEndpointTest 示例](http://web.archive.org/web/20221117050521/https://github.com/devdojobr/springboot-essentials/blob/master/src/test/java/br/com/devdojo/StudentEndpointTest.java)

<input type="hidden" id="mkyong-current-postId" value="14931">