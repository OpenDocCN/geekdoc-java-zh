# Spring Boot——Jetty 作为嵌入式服务器

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-jetty-as-embedded-server/>

默认情况下，Spring Boot 使用 Tomcat 作为默认的嵌入式服务器，要将其更改为 Jetty，只需排除 Tomcat 并包含 Jetty，如下所示:

## 1.弹簧靴起动器网

pom.xml

```java
 <dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
		<exclusions>
			<exclusion>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-tomcat</artifactId>
			</exclusion>
		</exclusions>
	</dependency>

	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-jetty</artifactId>
	</dependency> 
```

## 2.百里香叶

pom.xml

```java
 <dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-thymeleaf</artifactId>
		<exclusions>
			<exclusion>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-tomcat</artifactId>
			</exclusion>
		</exclusions>
	</dependency>

	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-jetty</artifactId>
	</dependency> 
```

查看依赖关系

```java
 $ mvn dependency:tree

[INFO] org.springframework.boot:spring-boot-web-thymeleaf:jar:1.0
[INFO] +- org.springframework.boot:spring-boot-starter-thymeleaf:jar:1.4.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:1.4.2.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-starter-logging:jar:1.4.2.RELEASE:compile
[INFO] |  |  |  +- ch.qos.logback:logback-classic:jar:1.1.7:compile
[INFO] |  |  |  |  \- ch.qos.logback:logback-core:jar:1.1.7:compile
[INFO] |  |  |  +- org.slf4j:jcl-over-slf4j:jar:1.7.21:compile
[INFO] |  |  |  +- org.slf4j:jul-to-slf4j:jar:1.7.21:compile
[INFO] |  |  |  \- org.slf4j:log4j-over-slf4j:jar:1.7.21:compile
[INFO] |  |  +- org.springframework:spring-core:jar:4.3.4.RELEASE:compile
[INFO] |  |  \- org.yaml:snakeyaml:jar:1.17:runtime
[INFO] |  +- org.springframework.boot:spring-boot-starter-web:jar:1.4.2.RELEASE:compile
[INFO] |  |  +- org.hibernate:hibernate-validator:jar:5.2.4.Final:compile
[INFO] |  |  |  +- javax.validation:validation-api:jar:1.1.0.Final:compile
[INFO] |  |  |  +- org.jboss.logging:jboss-logging:jar:3.3.0.Final:compile
[INFO] |  |  |  \- com.fasterxml:classmate:jar:1.3.3:compile
[INFO] |  |  +- com.fasterxml.jackson.core:jackson-databind:jar:2.8.4:compile
[INFO] |  |  |  +- com.fasterxml.jackson.core:jackson-annotations:jar:2.8.4:compile
[INFO] |  |  |  \- com.fasterxml.jackson.core:jackson-core:jar:2.8.4:compile
[INFO] |  |  +- org.springframework:spring-web:jar:4.3.4.RELEASE:compile
[INFO] |  |  |  +- org.springframework:spring-aop:jar:4.3.4.RELEASE:compile
[INFO] |  |  |  \- org.springframework:spring-beans:jar:4.3.4.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-webmvc:jar:4.3.4.RELEASE:compile
[INFO] |  |     \- org.springframework:spring-expression:jar:4.3.4.RELEASE:compile
[INFO] |  +- org.thymeleaf:thymeleaf-spring4:jar:2.1.5.RELEASE:compile
[INFO] |  |  +- org.thymeleaf:thymeleaf:jar:2.1.5.RELEASE:compile
[INFO] |  |  |  +- ognl:ognl:jar:3.0.8:compile
[INFO] |  |  |  +- org.javassist:javassist:jar:3.20.0-GA:compile
[INFO] |  |  |  \- org.unbescape:unbescape:jar:1.1.0.RELEASE:compile
[INFO] |  |  \- org.slf4j:slf4j-api:jar:1.7.21:compile
[INFO] |  \- nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect:jar:1.4.0:compile
[INFO] |     \- org.codehaus.groovy:groovy:jar:2.4.7:compile
[INFO] +- org.springframework.boot:spring-boot-starter-jetty:jar:1.4.2.RELEASE:compile
[INFO] |  +- org.eclipse.jetty:jetty-servlets:jar:9.3.14.v20161028:compile
[INFO] |  |  +- org.eclipse.jetty:jetty-continuation:jar:9.3.14.v20161028:compile
[INFO] |  |  +- org.eclipse.jetty:jetty-http:jar:9.3.14.v20161028:compile
[INFO] |  |  +- org.eclipse.jetty:jetty-util:jar:9.3.14.v20161028:compile
[INFO] |  |  \- org.eclipse.jetty:jetty-io:jar:9.3.14.v20161028:compile
[INFO] |  +- org.eclipse.jetty:jetty-webapp:jar:9.3.14.v20161028:compile
[INFO] |  |  +- org.eclipse.jetty:jetty-xml:jar:9.3.14.v20161028:compile
[INFO] |  |  \- org.eclipse.jetty:jetty-servlet:jar:9.3.14.v20161028:compile
[INFO] |  |     \- org.eclipse.jetty:jetty-security:jar:9.3.14.v20161028:compile
[INFO] |  |        \- org.eclipse.jetty:jetty-server:jar:9.3.14.v20161028:compile
[INFO] |  +- org.eclipse.jetty.websocket:websocket-server:jar:9.3.14.v20161028:compile
[INFO] |  |  +- org.eclipse.jetty.websocket:websocket-common:jar:9.3.14.v20161028:compile
[INFO] |  |  |  \- org.eclipse.jetty.websocket:websocket-api:jar:9.3.14.v20161028:compile
[INFO] |  |  +- org.eclipse.jetty.websocket:websocket-client:jar:9.3.14.v20161028:compile
[INFO] |  |  \- org.eclipse.jetty.websocket:websocket-servlet:jar:9.3.14.v20161028:compile
[INFO] |  |     \- javax.servlet:javax.servlet-api:jar:3.1.0:compile
[INFO] |  +- org.eclipse.jetty.websocket:javax-websocket-server-impl:jar:9.3.14.v20161028:compile
[INFO] |  |  +- org.eclipse.jetty:jetty-annotations:jar:9.3.14.v20161028:compile
[INFO] |  |  |  +- org.eclipse.jetty:jetty-plus:jar:9.3.14.v20161028:compile
[INFO] |  |  |  +- javax.annotation:javax.annotation-api:jar:1.2:compile
[INFO] |  |  |  +- org.ow2.asm:asm:jar:5.0.1:compile
[INFO] |  |  |  \- org.ow2.asm:asm-commons:jar:5.0.1:compile
[INFO] |  |  |     \- org.ow2.asm:asm-tree:jar:5.0.1:compile
[INFO] |  |  +- org.eclipse.jetty.websocket:javax-websocket-client-impl:jar:9.3.14.v20161028:compile
[INFO] |  |  \- javax.websocket:javax.websocket-api:jar:1.0:compile
[INFO] |  \- org.mortbay.jasper:apache-el:jar:8.0.33:compile
[INFO] +- org.springframework.boot:spring-boot-devtools:jar:1.4.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot:jar:1.4.2.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-context:jar:4.3.4.RELEASE:compile
[INFO] |  \- org.springframework.boot:spring-boot-autoconfigure:jar:1.4.2.RELEASE:compile 
```

*PS 用 Spring Boot 1.4.2.RELEASE 测试*

**Note**
Spring Boot 1.4.2.RELEASE use Jetty 9.3.14.v20161028

## 参考

1.  [Spring Boot——嵌入式 servlet 容器](http://web.archive.org/web/20220815221144/https://docs.spring.io/spring-boot/docs/current/reference/html/howto-embedded-servlet-containers.html)
2.  [码头服务器](http://web.archive.org/web/20220815221144/https://www.eclipse.org/jetty/)

<input type="hidden" id="mkyong-current-postId" value="14224">