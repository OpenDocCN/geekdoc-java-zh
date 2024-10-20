# Spring Boot +春季数据 JPA + PostgreSQL

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-spring-data-jpa-postgresql/>

![spring boot spring data jpa postgresql](img/dc67b8f31f8d26d043212864c7cd87cf.png)

之前的 [Spring Boot + Spring 数据 JPA](/web/20220930232242/https://mkyong.com/spring-boot/spring-boot-spring-data-jpa/) 将被重用，修改后支持 PostgreSQL 数据库。

使用的技术:

*   Spring Boot 2.1.2 .版本
*   弹簧 5.1.4 释放
*   休眠 5.3.7
*   HikariCP 3.2.0
*   PostgreSQL 驱动程序 42.2.5
*   maven3
*   Java 8

放置一个`postgresql`驱动程序，并在`application.properties`中定义数据源 url。完成后，Spring Boot 就可以连接到 PostgreSQL 数据库了。

pom.xml

```java
 <dependency>
		<groupId>org.postgresql</groupId>
		<artifactId>postgresql</artifactId>
	</dependency> 
```

## 1.专家

pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <artifactId>spring-boot-data-jpa</artifactId>
    <packaging>jar</packaging>
    <name>Spring Boot Spring Data JPA</name>
    <version>1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.2.RELEASE</version>
    </parent>

    <properties>
        <java.version>1.8</java.version>
        <downloadSources>true</downloadSources>
        <downloadJavadocs>true</downloadJavadocs>
    </properties>

    <dependencies>

        <!-- jpa, crud repository -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- PostgreSQL -->
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>

            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
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

## 2.配置 PostgreSQL

2.1 更新`spring.datasource.*`中的 PostgreSQL 设置

application.properties

```java
 ## default connection pool
spring.datasource.hikari.connectionTimeout=20000
spring.datasource.hikari.maximumPoolSize=5

## PostgreSQL
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=password

#drop n create table again, good for testing, comment this in production
spring.jpa.hibernate.ddl-auto=create 
```

完成了。

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20220930232242/https://github.com/mkyong/spring-boot.git)
$ cd spring-data-jpa-postgresql
$ mvn spring-boot:run

## 参考

*   [PostgreSQL](http://web.archive.org/web/20220930232242/https://www.postgresql.org/)
*   [鼠标指针](http://web.archive.org/web/20220930232242/https://github.com/brettwooldridge/HikariCP)
*   [春季数据 JPA 文档](http://web.archive.org/web/20220930232242/https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
*   [使用 SQL 数据库](http://web.archive.org/web/20220930232242/https://docs.spring.io/spring-boot/docs/2.1.3.RELEASE/reference/htmlsingle/#boot-features-sql)
*   [常用应用属性](http://web.archive.org/web/20220930232242/https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html)
*   [Spring Boot +春季数据 JPA](/web/20220930232242/https://mkyong.com/spring-boot/spring-boot-spring-data-jpa/)

<input type="hidden" id="mkyong-current-postId" value="14989">