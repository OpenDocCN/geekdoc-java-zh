# Spring Boot-应用程序启动时运行代码

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-run-code-when-the-application-starts/>

在 Spring Boot，当应用程序完全启动时，我们可以创建一个`CommandLineRunner` bean 来运行代码。

StartBookApplication.java

```java
 package com.mkyong;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

@SpringBootApplication
public class StartBookApplication {

    public static void main(String[] args) {
        SpringApplication.run(StartBookApplication.class, args);
    }

	// Injects a repository bean
    @Bean
    CommandLineRunner initDatabase(BookRepository repository) {
        return args -> {

            repository.save(
				new Book("A Guide to the Bodhisattva Way of Life")
			);

        };
    }

	// Injects a ApplicationContext
    @Bean
    CommandLineRunner initPrint(ApplicationContext ctx) {
        return args -> {

            System.out.println("All beans loaded by Spring Boot:\n");

            List<String> beans = Arrays.stream(ctx.getBeanDefinitionNames())
				.sorted(Comparator.naturalOrder())
				.collect(Collectors.toList());

            beans.forEach(x -> System.out.println(x));

        };
    }

	// Injects nothing
    @Bean
    CommandLineRunner initPrintOneLine() {
        return args -> {

            System.out.println("Hello World Spring Boot!");

        };
    }

} 
```

用 Spring Boot 2 号进行了测试

## 参考

*   [命令行运行程序](http://web.archive.org/web/20230101151418/https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/CommandLineRunner.html)

<input type="hidden" id="mkyong-current-postId" value="14955">