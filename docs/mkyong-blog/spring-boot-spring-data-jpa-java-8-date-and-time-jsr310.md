# Spring Boot +春季数据 JPA + Java 8 日期和时间(JSR310)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-spring-data-jpa-java-8-date-and-time-jsr310/>

在 Spring Boot + Spring Data JPA 应用程序中，为了支持 JSR 310`java.time.*`API，我们需要手动注册这个`Jsr310JpaConverters`。

```java
 import org.springframework.boot.autoconfigure.domain.EntityScan;
import org.springframework.data.jpa.convert.threeten.Jsr310JpaConverters;

@EntityScan(
        basePackageClasses = {Application.class, Jsr310JpaConverters.class}
)
@SpringBootApplication
public class Application {
	//...
} 
```

*用 Spring Boot 1.5.1 .版本测试的 PS，Spring Data JPA 1.11.0 .版本*

## 1.完整示例

1.1 一个模型包含一个`java.time.LocalDate`字段。

```java
 package com.mkyong.model;

import javax.persistence.*;
import java.time.LocalDate;

@Entity
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "CUST_SEQ")
    @SequenceGenerator(sequenceName = "customer_seq", allocationSize = 1, name = "CUST_SEQ")
    Long id;

    String name;

    @Column(name = "CREATED_DATE")
    LocalDate date;

	//... 
```

1.2 `@EntityScan`扫描并记录`Jsr310JpaConverters`如下:

```java
 package com.mkyong;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.domain.EntityScan;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.data.jpa.convert.threeten.Jsr310JpaConverters;

import java.util.Arrays;

//for jsr310 java 8 java.time.*
@EntityScan(
        basePackageClasses = {Application.class, Jsr310JpaConverters.class}
)
@SpringBootApplication
public class Application {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public CommandLineRunner run(ApplicationContext appContext) {
        return args -> {

            System.out.println("hello World!");

        };
    }

} 
```

## 参考

1.  [JSR 310 JPA converters JavaDoc](http://web.archive.org/web/20220930231922/https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/convert/threeten/Jsr310JpaConverters.html)
2.  [JSR 310:日期和时间 API](http://web.archive.org/web/20220930231922/https://jcp.org/en/jsr/detail?id=310)
3.  [Spring Data MongoDB + JSR-310 或 Java 8 新日期 API](http://web.archive.org/web/20220930231922/https://www.mkyong.com/mongodb/spring-data-mongodb-jsr-310-or-java-8-new-date-apis/)

<input type="hidden" id="mkyong-current-postId" value="14472">