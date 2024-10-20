# Spring Boot +春季数据 JPA

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-spring-data-jpa/>

![spring boot spring data jpa](img/2ea3e85d97109c13718e8c2178649a35.png)

在本教程中，我们将向您展示如何使用 Spring Boot + Spring data JPA 将数据保存到 H2 内存数据库中，以及如何查询数据。

使用的技术:

*   Spring Boot 2.1.2 .版本
*   弹簧 5.1.4 释放
*   休眠 5.3.7
*   HikariCP 3.2.0
*   H2 内存数据库 1.4.197
*   maven3
*   Java 8

**Note**
By default, Spring Boot 2 uses [HikariCP](http://web.archive.org/web/20220504210333/https://github.com/brettwooldridge/HikariCP) as the database connection pool. Read this [Spring Boot algorithm](http://web.archive.org/web/20220504210333/https://docs.spring.io/spring-boot/docs/2.1.3.RELEASE/reference/htmlsingle/#boot-features-connect-to-production-database) to choose a pool implementation.

## 1.项目目录

![project directory](img/a4b4c8d524ae7acbbae09432e18baa3f.png)

## 2.专家

放置`spring-boot-starter-data-jpa`，它将获得 Spring 数据、Hibernate、HikariCP 和所有数据库相关的依赖项。

pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <artifactId>spring-data-jpa</artifactId>
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

        <!-- in-memory database  -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
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

显示项目依赖关系。

```java
 $ mvn dependency:tree

[INFO] org.springframework.boot:spring-boot-data-jpa:jar:1.0
[INFO] +- org.springframework.boot:spring-boot-starter-data-jpa:jar:2.1.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-aop:jar:2.1.2.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-aop:jar:5.1.4.RELEASE:compile
[INFO] |  |  \- org.aspectj:aspectjweaver:jar:1.9.2:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-jdbc:jar:2.1.2.RELEASE:compile
[INFO] |  |  +- com.zaxxer:HikariCP:jar:3.2.0:compile
[INFO] |  |  \- org.springframework:spring-jdbc:jar:5.1.4.RELEASE:compile
[INFO] |  +- javax.transaction:javax.transaction-api:jar:1.3:compile
[INFO] |  +- javax.xml.bind:jaxb-api:jar:2.3.1:compile
[INFO] |  |  \- javax.activation:javax.activation-api:jar:1.2.0:compile
[INFO] |  +- org.hibernate:hibernate-core:jar:5.3.7.Final:compile
[INFO] |  |  +- org.jboss.logging:jboss-logging:jar:3.3.2.Final:compile
[INFO] |  |  +- javax.persistence:javax.persistence-api:jar:2.2:compile
[INFO] |  |  +- org.javassist:javassist:jar:3.23.1-GA:compile
[INFO] |  |  +- net.bytebuddy:byte-buddy:jar:1.9.7:compile
[INFO] |  |  +- antlr:antlr:jar:2.7.7:compile
[INFO] |  |  +- org.jboss:jandex:jar:2.0.5.Final:compile
[INFO] |  |  +- com.fasterxml:classmate:jar:1.4.0:compile
[INFO] |  |  +- org.dom4j:dom4j:jar:2.1.1:compile
[INFO] |  |  \- org.hibernate.common:hibernate-commons-annotations:jar:5.0.4.Final:compile
[INFO] |  +- org.springframework.data:spring-data-jpa:jar:2.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework.data:spring-data-commons:jar:2.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-orm:jar:5.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-context:jar:5.1.4.RELEASE:compile
[INFO] |  |  |  \- org.springframework:spring-expression:jar:5.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-tx:jar:5.1.4.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-beans:jar:5.1.4.RELEASE:compile
[INFO] |  |  \- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO] |  \- org.springframework:spring-aspects:jar:5.1.4.RELEASE:compile
[INFO] +- com.h2database:h2:jar:1.4.197:compile
[INFO] \- org.springframework.boot:spring-boot-starter-test:jar:2.1.2.RELEASE:test
[INFO]    +- org.springframework.boot:spring-boot-starter:jar:2.1.2.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot:jar:2.1.2.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot-autoconfigure:jar:2.1.2.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot-starter-logging:jar:2.1.2.RELEASE:compile
[INFO]    |  |  +- ch.qos.logback:logback-classic:jar:1.2.3:compile
[INFO]    |  |  |  \- ch.qos.logback:logback-core:jar:1.2.3:compile
[INFO]    |  |  +- org.apache.logging.log4j:log4j-to-slf4j:jar:2.11.1:compile
[INFO]    |  |  |  \- org.apache.logging.log4j:log4j-api:jar:2.11.1:compile
[INFO]    |  |  \- org.slf4j:jul-to-slf4j:jar:1.7.25:compile
[INFO]    |  +- javax.annotation:javax.annotation-api:jar:1.3.2:compile
[INFO]    |  \- org.yaml:snakeyaml:jar:1.23:runtime
[INFO]    +- org.springframework.boot:spring-boot-test:jar:2.1.2.RELEASE:test
[INFO]    +- org.springframework.boot:spring-boot-test-autoconfigure:jar:2.1.2.RELEASE:test
[INFO]    +- com.jayway.jsonpath:json-path:jar:2.4.0:test
[INFO]    |  \- net.minidev:json-smart:jar:2.3:test
[INFO]    |     \- net.minidev:accessors-smart:jar:1.2:test
[INFO]    |        \- org.ow2.asm:asm:jar:5.0.4:test
[INFO]    +- junit:junit:jar:4.12:test
[INFO]    +- org.assertj:assertj-core:jar:3.11.1:test
[INFO]    +- org.mockito:mockito-core:jar:2.23.4:test
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
```

## 3.春季数据 JPA

Book.java

```java
 package com.mkyong;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    private String name;

    public Book() {
    }

    public Book(String name) {
        this.name = name;
    }

    //getters, setters, toString...
} 
```

BookRepository.java

```java
 package com.mkyong;

import org.springframework.data.repository.CrudRepository;

import java.util.List;

public interface BookRepository extends CrudRepository<Book, Long> {

    List<Book> findByName(String name);

} 
```

## 4.@SpringBootApplication

4.1 启动 Spring Boot，将 3 本书插入 H2 数据库，并测试`find()`方法。

StartApplication.java

```java
 package com.mkyong;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StartApplication implements CommandLineRunner {

    private static final Logger log = LoggerFactory.getLogger(StartApplication.class);

    @Autowired
    private BookRepository repository;

    public static void main(String[] args) {
        SpringApplication.run(StartApplication.class, args);
    }

    @Override
    public void run(String... args) {

        log.info("StartApplication...");

        repository.save(new Book("Java"));
        repository.save(new Book("Node"));
        repository.save(new Book("Python"));

        System.out.println("\nfindAll()");
        repository.findAll().forEach(x -> System.out.println(x));

        System.out.println("\nfindById(1L)");
        repository.findById(1l).ifPresent(x -> System.out.println(x));

        System.out.println("\nfindByName('Node')");
        repository.findByName("Node").forEach(x -> System.out.println(x));

    }

} 
```

## 5.HikariCP 连接池

默认情况下，Spring Boot 2 使用 HikariCP 作为默认连接池。

5.1 启用`com.zaxxer`的日志级别进行调试，它将打印出默认的 HikariCP 配置。

application.properties

```java
 logging.level.org.springframework=INFO
logging.level.com.mkyong=INFO
logging.level.com.zaxxer=DEBUG
logging.level.root=ERROR 
```

5.2 运行 Spring Boot，检查测井记录:

```java
 DEBUG com.zaxxer.hikari.HikariConfig - Driver class org.h2.Driver found in Thread context class loader jdk.internal.loader.ClassLoaders$AppClassLoader@28c97a5
DEBUG com.zaxxer.hikari.HikariConfig - HikariPool-1 - configuration:
DEBUG com.zaxxer.hikari.HikariConfig - allowPoolSuspension.............false
DEBUG com.zaxxer.hikari.HikariConfig - autoCommit......................true
DEBUG com.zaxxer.hikari.HikariConfig - catalog.........................none
DEBUG com.zaxxer.hikari.HikariConfig - connectionInitSql...............none
DEBUG com.zaxxer.hikari.HikariConfig - connectionTestQuery.............none
DEBUG com.zaxxer.hikari.HikariConfig - connectionTimeout...............30000
DEBUG com.zaxxer.hikari.HikariConfig - dataSource......................none
DEBUG com.zaxxer.hikari.HikariConfig - dataSourceClassName.............none
DEBUG com.zaxxer.hikari.HikariConfig - dataSourceJNDI..................none
DEBUG com.zaxxer.hikari.HikariConfig - dataSourceProperties............{password=<masked>}
DEBUG com.zaxxer.hikari.HikariConfig - driverClassName................."org.h2.Driver"
DEBUG com.zaxxer.hikari.HikariConfig - healthCheckProperties...........{}
DEBUG com.zaxxer.hikari.HikariConfig - healthCheckRegistry.............none
DEBUG com.zaxxer.hikari.HikariConfig - idleTimeout.....................600000
DEBUG com.zaxxer.hikari.HikariConfig - initializationFailTimeout.......1
DEBUG com.zaxxer.hikari.HikariConfig - isolateInternalQueries..........false
DEBUG com.zaxxer.hikari.HikariConfig - jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
DEBUG com.zaxxer.hikari.HikariConfig - leakDetectionThreshold..........0
DEBUG com.zaxxer.hikari.HikariConfig - maxLifetime.....................1800000
DEBUG com.zaxxer.hikari.HikariConfig - maximumPoolSize.................10
DEBUG com.zaxxer.hikari.HikariConfig - metricRegistry..................none
DEBUG com.zaxxer.hikari.HikariConfig - metricsTrackerFactory...........none
DEBUG com.zaxxer.hikari.HikariConfig - minimumIdle.....................10
DEBUG com.zaxxer.hikari.HikariConfig - password........................<masked>
DEBUG com.zaxxer.hikari.HikariConfig - poolName........................"HikariPool-1"
DEBUG com.zaxxer.hikari.HikariConfig - readOnly........................false
DEBUG com.zaxxer.hikari.HikariConfig - registerMbeans..................false
DEBUG com.zaxxer.hikari.HikariConfig - scheduledExecutor...............none
DEBUG com.zaxxer.hikari.HikariConfig - schema..........................none
DEBUG com.zaxxer.hikari.HikariConfig - threadFactory...................internal
DEBUG com.zaxxer.hikari.HikariConfig - transactionIsolation............default
DEBUG com.zaxxer.hikari.HikariConfig - username........................"sa"
DEBUG com.zaxxer.hikari.HikariConfig - validationTimeout...............5000
INFO  com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Starting...
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn0: url=jdbc:h2:mem:testdb user=SA
INFO  com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Start completed.
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Pool stats (total=1, active=0, idle=1, waiting=0)
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn1: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn2: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn3: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn4: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn5: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn6: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn7: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn8: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn9: url=jdbc:h2:mem:testdb user=SA
DEBUG com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - After adding stats (total=10, active=0, idle=10, waiting=0) 
```

5.2 我们可以使用`spring.datasource.hikari.*`来覆盖默认设置。

application.properties

```java
 logging.level.org.springframework=INFO
logging.level.com.mkyong=INFO
logging.level.com.zaxxer=DEBUG
logging.level.root=ERROR

spring.datasource.hikari.connectionTimeout=20000
spring.datasource.hikari.maximumPoolSize=5
spring.datasource.hikari.poolName=HikariPoolZZZ 
```

## 6.单元测试

6.1 `@DataJpaTest`和`TestEntityManager`测试 Spring 数据 JPA 存储库。

BookRepositoryTest.java

```java
 package com.mkyong;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.List;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.Assert.assertEquals;

@RunWith(SpringRunner.class)
@DataJpaTest
public class BookRepositoryTest {

    @Autowired
    private TestEntityManager entityManager;

    @Autowired
    private BookRepository repository;

    @Test
    public void testFindByName() {

        entityManager.persist(new Book("C++"));

        List<Book> books = repository.findByName("C++");
        assertEquals(1, books.size());

        assertThat(books).extracting(Book::getName).containsOnly("C++");

    }

} 
```

## 7.演示

```java
 $ mvn spring-boot:run

INFO  com.mkyong.StartApplication - Started StartApplication in 2.79 seconds (JVM running for 10.883)
INFO  com.mkyong.StartApplication - StartApplication...

findAll()
DEBUG com.zaxxer.hikari.pool.PoolBase - HikariPoolZZZ - Reset (readOnly) on connection conn0: url=jdbc:h2:mem:testdb user=SA
Book{id=1, name='Java'}
Book{id=2, name='Node'}
Book{id=3, name='Python'}

findById(1L)
DEBUG com.zaxxer.hikari.pool.PoolBase - HikariPoolZZZ - Reset (readOnly) on connection conn0: url=jdbc:h2:mem:testdb user=SA
Book{id=1, name='Java'}

findByName('Node')
Book{id=2, name='Node'} 
```

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20220504210333/https://github.com/mkyong/spring-boot.git)
$ cd spring-data-jpa
$ mvn spring-boot:run

## 参考

*   [鼠标指针](http://web.archive.org/web/20220504210333/https://github.com/brettwooldridge/HikariCP)
*   [春季数据 JPA 文档](http://web.archive.org/web/20220504210333/https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
*   [使用 SQL 数据库](http://web.archive.org/web/20220504210333/https://docs.spring.io/spring-boot/docs/2.1.3.RELEASE/reference/htmlsingle/#boot-features-sql)
*   [常用应用属性](http://web.archive.org/web/20220504210333/https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html)
*   [JUnit–如何测试列表](http://web.archive.org/web/20220504210333/https://www.mkyong.com/unittest/junit-how-to-test-a-list/)
*   [DataJpaTest 多克](http://web.archive.org/web/20220504210333/https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/orm/jpa/DataJpaTest.html)
*   [TestEntityManager 文档](http://web.archive.org/web/20220504210333/https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/orm/jpa/TestEntityManager.html)
*   [Spring Boot+Spring Data JPA+Oracle 示例](http://web.archive.org/web/20220504210333/https://www.mkyong.com/spring-boot/spring-boot-spring-data-jpa-oracle-example/)
*   [Spring Boot 通过 JPA 访问数据](http://web.archive.org/web/20220504210333/https://spring.io/guides/gs/accessing-data-jpa/)

<input type="hidden" id="mkyong-current-postId" value="14983">