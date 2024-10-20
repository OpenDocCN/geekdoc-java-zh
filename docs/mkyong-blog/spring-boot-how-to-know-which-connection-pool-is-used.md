# Spring Boot——如何知道使用了哪个连接池？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-how-to-know-which-connection-pool-is-used/>

在 Spring Boot，`@Autowired` a `javax.sql.DataSource`，你就会知道当前运行的应用程序使用的是哪个数据库连接池。

## 1.测试默认值

Spring Boot 举例打印一份`javax.sql.DataSource`

**Note**
Read this official Spring Boot doc – [Connection to a production database](http://web.archive.org/web/20190223075556/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-sql.html#boot-features-connect-to-production-database), to understand the algorithm for choosing a `DataSource` implementations – Tomcat pooling, HikariCP, Commons DBCP and Commons DBCP2.

```java
 package com.mkyong;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import javax.sql.DataSource;

@SpringBootApplication
public class SpringBootConsoleApplication implements CommandLineRunner {

    @Autowired
    DataSource dataSource;

    public static void main(String[] args) throws Exception {
        SpringApplication.run(SpringBootConsoleApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {

        System.out.println("DATASOURCE = " + dataSource);

    }

} 
```

输出，Spring Boot 默认使用 Tomcat 池。

```java
 DATASOURCE = org.apache.tomcat.jdbc.pool.DataSource@7c541c15... 
```

 ## 2.hikaricp 测试

要切换到另一个连接池，例如 HikariCP，只需排除默认连接池，并将 HikariCP 包含在类路径中。

pom.xml

```java
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.tomcat</groupId>
                    <artifactId>tomcat-jdbc</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!-- connection pools -->
        <dependency>
            <groupId>com.zaxxer</groupId>
            <artifactId>HikariCP</artifactId>
            <version>2.6.0</version>
        </dependency> 
```

输出

```java
 DATASOURCE = HikariDataSource (HikariPool-1) 
```

**Note**
Read this example – [Spring Boot JDBC + MySQL + HikariCP example](http://web.archive.org/web/20190223075556/http://www.mkyong.com/spring-boot/spring-boot-jdbc-mysql-hikaricp-example/) ## 参考

1.  [鼠标指针](http://web.archive.org/web/20190223075556/https://github.com/brettwooldridge/HikariCP)
2.  [Spring Boot JDBC+MySQL+HikariCP 的例子](http://web.archive.org/web/20190223075556/http://www.mkyong.com/spring-boot/spring-boot-jdbc-mysql-hikaricp-example/)

[connection pool](http://web.archive.org/web/20190223075556/http://www.mkyong.com/tag/connection-pool/) [hikaricp](http://web.archive.org/web/20190223075556/http://www.mkyong.com/tag/hikaricp/) [spring boot](http://web.archive.org/web/20190223075556/http://www.mkyong.com/tag/spring-boot/) [tomcat pool](http://web.archive.org/web/20190223075556/http://www.mkyong.com/tag/tomcat-pool/)







