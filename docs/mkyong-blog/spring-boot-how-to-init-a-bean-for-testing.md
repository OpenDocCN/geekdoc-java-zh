# Spring Boot——如何初始化用于测试的 Bean？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-how-to-init-a-bean-for-testing/>

在 Spring Boot，我们可以创建一个`@TestConfiguration`类来初始化一些只用于测试类的 beans。

用 Spring Boot 2 号进行了测试

## 1.@测试配置+@导入

这个`@TestConfiguration`类不会被组件扫描选中，我们需要手动导入它。

TestConfig.java

```java
 package com.mkyong;

import org.springframework.boot.test.context.TestConfiguration;
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.context.annotation.Bean;

import java.time.Duration;

@TestConfiguration
public class TestConfig {

    @Bean
    public RestTemplateBuilder restTemplateBuilder() {

        return new RestTemplateBuilder()
                .basicAuthentication("mkyong", "password")
                .setConnectTimeout(Duration.ofSeconds(5));
    }
} 
```

RestTemplateTest.java

```java
 package com.mkyong;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.context.annotation.Import;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
@Import(TestConfig.class)
public class RestTemplateTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    public void post_user_ok() {
        //...
    }

} 
```

## 2.@TestConfiguration +内部静态类

或者，创建一个内部类，如下所示:

RestTemplateTest.java

```java
 package com.mkyong;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.context.TestConfiguration;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.test.context.junit4.SpringRunner;

import java.time.Duration;

@RunWith(SpringRunner.class)
@SpringBootTest
public class RestTemplateTest {

    @TestConfiguration
    static class TestConfig {

        @Bean
        public RestTemplateBuilder restTemplateBuilder() {

            return new RestTemplateBuilder()
                    .basicAuthentication("mkyong", "password")
                    .setConnectTimeout(Duration.ofSeconds(5));
        }

    }

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    public void post_user_ok() {
        //...
    }

} 
```

## 参考

*   [测试 Spring Boot 应用](http://web.archive.org/web/20221201060348/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications)

<input type="hidden" id="mkyong-current-postId" value="14940">