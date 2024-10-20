# Spring Boot WebFlux +百里香叶反应实例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-webflux-thymeleaf-reactive-example/>

在本文中，我们将向您展示如何开发一个反应式 web 应用程序。

*   Spring Boot 2.1.2 .版本
*   Spring WebFlux 5.1.4 .发行版
*   百里香叶
*   maven3

Spring Boot 将配置一切，关键是使用百里香叶`ReactiveDataDriverContextVariable`来启用百里香叶模板中的数据驱动模式。

```java
 @RequestMapping("/")
    public String index(final Model model) {

        // data streaming, data driven mode.
        IReactiveDataDriverContextVariable reactiveDataDrivenMode =
                new ReactiveDataDriverContextVariable(movieRepository.findAll(), 1);

        model.addAttribute("movies", reactiveDataDrivenMode);

        return "view";

    } 
```

Note
Working on the Spring Boot WebFlux + Thymeleaf + Server-Sent Events (SSE) integeration. To be updated here.

## 1.项目目录

![project directory](img/4776949d6f6d9e42185905f1a9c70b85.png)

## 2.专家

pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
	 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mkyong.spring.reactive</groupId>
    <artifactId>webflux-thymeleaf</artifactId>
    <version>1.0</version>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.2.RELEASE</version>
    </parent>

    <dependencies>

        <!-- reactive -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-webflux</artifactId>
        </dependency>

        <!-- just include the normal thymeleaf -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

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
            </plugin>
        </plugins>
    </build>

</project> 
```

显示项目依赖关系。

```java
 $ mvn dependency:tree

[INFO] com.mkyong.spring.webflux:ReactiveApp:jar:1.0-SNAPSHOT
[INFO] +- org.springframework.boot:spring-boot-starter-webflux:jar:2.1.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:2.1.2.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot:jar:2.1.2.RELEASE:compile
[INFO] |  |  |  \- org.springframework:spring-context:jar:5.1.4.RELEASE:compile
[INFO] |  |  |     +- org.springframework:spring-aop:jar:5.1.4.RELEASE:compile
[INFO] |  |  |     \- org.springframework:spring-expression:jar:5.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-autoconfigure:jar:2.1.2.RELEASE:compile
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
[INFO] |  +- org.springframework.boot:spring-boot-starter-reactor-netty:jar:2.1.2.RELEASE:compile
[INFO] |  |  \- io.projectreactor.netty:reactor-netty:jar:0.8.4.RELEASE:compile
[INFO] |  |     +- io.netty:netty-codec-http:jar:4.1.31.Final:compile
[INFO] |  |     |  \- io.netty:netty-codec:jar:4.1.31.Final:compile
[INFO] |  |     +- io.netty:netty-codec-http2:jar:4.1.31.Final:compile
[INFO] |  |     +- io.netty:netty-handler:jar:4.1.31.Final:compile
[INFO] |  |     |  +- io.netty:netty-buffer:jar:4.1.31.Final:compile
[INFO] |  |     |  \- io.netty:netty-transport:jar:4.1.31.Final:compile
[INFO] |  |     |     \- io.netty:netty-resolver:jar:4.1.31.Final:compile
[INFO] |  |     +- io.netty:netty-handler-proxy:jar:4.1.31.Final:compile
[INFO] |  |     |  \- io.netty:netty-codec-socks:jar:4.1.31.Final:compile
[INFO] |  |     \- io.netty:netty-transport-native-epoll:jar:linux-x86_64:4.1.31.Final:compile
[INFO] |  |        +- io.netty:netty-common:jar:4.1.31.Final:compile
[INFO] |  |        \- io.netty:netty-transport-native-unix-common:jar:4.1.31.Final:compile
[INFO] |  +- org.hibernate.validator:hibernate-validator:jar:6.0.14.Final:compile
[INFO] |  |  +- javax.validation:validation-api:jar:2.0.1.Final:compile
[INFO] |  |  +- org.jboss.logging:jboss-logging:jar:3.3.2.Final:compile
[INFO] |  |  \- com.fasterxml:classmate:jar:1.4.0:compile
[INFO] |  +- org.springframework:spring-web:jar:5.1.4.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-beans:jar:5.1.4.RELEASE:compile
[INFO] |  +- org.springframework:spring-webflux:jar:5.1.4.RELEASE:compile
[INFO] |  |  \- io.projectreactor:reactor-core:jar:3.2.5.RELEASE:compile
[INFO] |  |     \- org.reactivestreams:reactive-streams:jar:1.0.2:compile
[INFO] |  \- org.synchronoss.cloud:nio-multipart-parser:jar:1.1.0:compile
[INFO] |     +- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO] |     \- org.synchronoss.cloud:nio-stream-storage:jar:1.1.3:compile
[INFO] +- org.springframework.boot:spring-boot-starter-thymeleaf:jar:2.1.2.RELEASE:compile
[INFO] |  +- org.thymeleaf:thymeleaf-spring5:jar:3.0.11.RELEASE:compile
[INFO] |  |  \- org.thymeleaf:thymeleaf:jar:3.0.11.RELEASE:compile
[INFO] |  |     +- org.attoparser:attoparser:jar:2.0.5.RELEASE:compile
[INFO] |  |     \- org.unbescape:unbescape:jar:1.1.6.RELEASE:compile
[INFO] |  \- org.thymeleaf.extras:thymeleaf-extras-java8time:jar:3.0.2.RELEASE:compile
[INFO] \- org.springframework.boot:spring-boot-starter-test:jar:2.1.2.RELEASE:test
[INFO]    +- org.springframework.boot:spring-boot-test:jar:2.1.2.RELEASE:test
[INFO]    +- org.springframework.boot:spring-boot-test-autoconfigure:jar:2.1.2.RELEASE:test
[INFO]    +- com.jayway.jsonpath:json-path:jar:2.4.0:test
[INFO]    |  \- net.minidev:json-smart:jar:2.3:test
[INFO]    |     \- net.minidev:accessors-smart:jar:1.2:test
[INFO]    |        \- org.ow2.asm:asm:jar:5.0.4:test
[INFO]    +- junit:junit:jar:4.12:test
[INFO]    +- org.assertj:assertj-core:jar:3.11.1:test
[INFO]    +- org.mockito:mockito-core:jar:2.23.4:test
[INFO]    |  +- net.bytebuddy:byte-buddy:jar:1.9.7:test
[INFO]    |  +- net.bytebuddy:byte-buddy-agent:jar:1.9.7:test
[INFO]    |  \- org.objenesis:objenesis:jar:2.6:test
[INFO]    +- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO]    +- org.hamcrest:hamcrest-library:jar:1.3:test
[INFO]    +- org.skyscreamer:jsonassert:jar:1.5.0:test
[INFO]    |  \- com.vaadin.external.google:android-json:jar:0.0.20131108.vaadin1:test
[INFO]    +- org.springframework:spring-core:jar:5.1.4.RELEASE:compile
[INFO]    |  \- org.springframework:spring-jcl:jar:5.1.4.RELEASE:compile
[INFO]    +- org.springframework:spring-test:jar:5.1.4.RELEASE:test
[INFO]    \- org.xmlunit:xmlunit-core:jar:2.6.2:test
[INFO]       \- javax.xml.bind:jaxb-api:jar:2.3.1:test
[INFO]          \- javax.activation:javax.activation-api:jar:1.2.0:test 
```

## 3.Spring Boot +春网流量

3.1 基于 Spring WebFlux 注释的控制器。用`ReactiveDataDriverContextVariable`包装数据，将在百里香模板中启用反应式数据驱动模型。

MovieController.java

```java
 package com.mkyong.reactive.controller;

import com.mkyong.reactive.repository.MovieRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.thymeleaf.spring5.context.webflux.IReactiveDataDriverContextVariable;
import org.thymeleaf.spring5.context.webflux.ReactiveDataDriverContextVariable;

@Controller
public class MovieController {

    @Autowired
    private MovieRepository movieRepository;

    @RequestMapping("/")
    public String index(final Model model) {

        // loads 1 and display 1, stream data, data driven mode.
        IReactiveDataDriverContextVariable reactiveDataDrivenMode =
                new ReactiveDataDriverContextVariable(movieRepository.findAll(), 1);

        model.addAttribute("movies", reactiveDataDrivenMode);

        // classic, wait repository loaded all and display it.
        //model.addAttribute("movies", movieRepository.findAll());

        return "index";

    }

} 
```

3.2 在`repository`中，返回一个`Flux`对象。

MovieRepository.java

```java
 package com.mkyong.reactive.repository;

import com.mkyong.reactive.Movie;
import reactor.core.publisher.Flux;

public interface MovieRepository {

    Flux<Movie> findAll();

} 
```

ReactiveMovieRepository.java

```java
 package com.mkyong.reactive.repository;

import com.mkyong.reactive.Movie;
import org.springframework.stereotype.Repository;
import reactor.core.publisher.Flux;

import java.time.Duration;
import java.util.ArrayList;
import java.util.List;

@Repository
public class ReactiveMovieRepository implements MovieRepository {

    private static List<Movie> movie = new ArrayList<>();

    static {
        movie.add(new Movie("Polar (2019)", 64));
        movie.add(new Movie("Iron Man (2008)", 79));
        movie.add(new Movie("The Shawshank Redemption (1994)", 93));
        movie.add(new Movie("Forrest Gump (1994)", 83));
        movie.add(new Movie("Glass (2019)", 70));
    }

    @Override
    public Flux<Movie> findAll() {
        //Simulate big list of data, streaming it every 2 second delay
        return Flux.fromIterable(movie).delayElements(Duration.ofSeconds(2));
    }

} 
```

3.3 电影模式。

Movie.java

```java
 package com.mkyong.reactive;

public class Movie {

    private String name;
    private Integer score;

    //getter, setter and constructor
} 
```

3.4 启动 Spring Boot。

MovieWebApplication.java

```java
 package com.mkyong.reactive;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MovieWebApplication {

    public static void main(String[] args) {
        SpringApplication.run(MovieWebApplication.class, args);
    }

} 
```

## 4.百里香叶

百里香模板中没有特殊的反应标签，只是使用了普通的循环。

resources/templates/index.html

```java
 <!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link data-th-href="@{/css/bootstrap.min.css}" rel="stylesheet">
    <link data-th-href="@{/css/main.css}" rel="stylesheet">
</head>
<body>

<div class="container">

    <div class="row">
        <div id="title">
            <h1>Spring WebFlux + Thymeleaf</h1>
        </div>
        <table id="allMovies" class="table table-striped">
            <thead>
            <tr>
                <th width="70%">Name</th>
                <th>Score</th>
            </tr>
            </thead>
            <tbody>
            <tr class="result" data-th-each="movie : ${movies}">
                <td>[[${movie.name}]]</td>
                <td>[[${movie.score}]]</td>
            </tr>
            </tbody>
        </table>
    </div>

</div> 
```

定义了块大小。

application.properties

```java
 spring.thymeleaf.reactive.max-chunk-size=8192 
```

完成了。

## 5.演示

```java
 $ mvn spring-boot:run 
```

URL =*http://localhost:8080*
数据是流式的，将每 2 秒钟以反应方式显示一次。

![](img/8700e4ba5f9edc63bff2564cd79495bf.png)![](img/4e23e779182c84412cafa81f1c6c3e18.png)![](img/7db98f89adda8e94943d3ac6febf9dbd.png)![](img/c23555307ad5a8eef55f5b67912c774d.png)

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20220401135609/https://github.com/mkyong/spring-boot.git)
$ cd webflux-thymeleaf
$ mvn spring-boot:run

## 参考

*   [IReactiveDataDriverContextVariable](http://web.archive.org/web/20220401135609/https://www.thymeleaf.org/apidocs/thymeleaf-spring5/3.0.5.M3/org/thymeleaf/spring5/context/webflux/IReactiveDataDriverContextVariable.html)
*   [反应堆上的卷筒纸](http://web.archive.org/web/20220401135609/https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html)
*   [百里香叶与弹簧网反应](http://web.archive.org/web/20220401135609/https://github.com/thymeleaf/thymeleafsandbox-biglist-reactive)
*   [Spring WebFlux Workshop](http://web.archive.org/web/20220401135609/https://bclozel.github.io/webflux-workshop/)

<input type="hidden" id="mkyong-current-postId" value="14902">