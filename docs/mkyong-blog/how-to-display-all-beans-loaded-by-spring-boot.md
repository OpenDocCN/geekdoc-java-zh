# 如何显示 Spring Boot 加载的所有 beans

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/how-to-display-all-beans-loaded-by-spring-boot/>

在 Spring Boot，您可以使用`appContext.getBeanDefinitionNames()`来获取 Spring 容器装载的所有 beans。

## 1.CommandLineRunner 作为接口

Application.java

```java
 package com.mkyong;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

import java.util.Arrays;

@SpringBootApplication
public class Application implements CommandLineRunner {

    @Autowired
    private ApplicationContext appContext;

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }

    @Override
    public void run(String... args) throws Exception {

        String[] beans = appContext.getBeanDefinitionNames();
        Arrays.sort(beans);
        for (String bean : beans) {
            System.out.println(bean);
        }

    }

} 
```

输出

Console.java

```java
 application
customerRepository
customerRepositoryImpl
dataSource
dataSourceInitializedPublisher
dataSourceInitializer
dataSourceInitializerPostProcessor
emBeanDefinitionRegistrarPostProcessor
entityManagerFactory
entityManagerFactoryBuilder
hikariPoolDataSourceMetadataProvider
jdbcTemplate
jpaContext
//... 
```

 ## 2.作为 Bean 的 CommandLineRunner

只是打印加载的 beans 的方式不同。

Application.java

```java
 package com.mkyong;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;

import java.util.Arrays;

@SpringBootApplication
public class Application {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public CommandLineRunner run(ApplicationContext appContext) {
        return args -> {

            String[] beans = appContext.getBeanDefinitionNames();
            Arrays.stream(beans).sorted().forEach(System.out::println);

        };
    }

} 
```

 ## 参考

1.  [命令行运行程序 JavaDoc](http://web.archive.org/web/20190225100502/http://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/CommandLineRunner.html)
2.  [Spring Boot 非 web 应用实例](http://web.archive.org/web/20190225100502/http://www.mkyong.com/spring-boot/spring-boot-non-web-application-example/)

[application context](http://web.archive.org/web/20190225100502/http://www.mkyong.com/tag/application-context/) [spring boot](http://web.archive.org/web/20190225100502/http://www.mkyong.com/tag/spring-boot/)







