# Spring Boot web JSP–没有可用的 Java 编译器

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-web-jsp-no-java-compiler-available/>

内嵌 Tomcat 和 JSP 的 Spring boot web 应用程序。运行并访问 JSP 页面，但是遇到以下错误

```java
 $ mvn spring-boot:run
...
java.lang.IllegalStateException: No Java compiler available
        at org.apache.jasper.JspCompilationContext.createCompiler(JspCompilationContext.java:235) ~[tomcat-embed-jasper-8.5.6.jar:8.5.6]
        at org.apache.jasper.JspCompilationContext.compile(JspCompilationContext.java:592) ~[tomcat-embed-jasper-8.5.6.jar:8.5.6]
        at org.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:368) ~[tomcat-embed-jasper-8.5.6.jar:8.5.6]
        at org.apache.jasper.servlet.JspServlet.serviceJspFile(JspServlet.java:385) ~[tomcat-embed-jasper-8.5.6.jar:8.5.6]
        at org.apache.jasper.servlet.JspServlet.service(JspServlet.java:329) ~[tomcat-embed-jasper-8.5.6.jar:8.5.6]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:729) [tomcat-embed-core-8.5.6.jar:8.5.6]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:230) [tomcat-embed-core-8.5.6.jar:8.5.6]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.6.jar:8.5.6]
        at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52) [tomcat-embed-websocket-8.5.6.jar:8.5.6]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.6.jar:8.5.6]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.6.jar:8.5.6]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:101) [spring-web-4.3.4.RELEASE.jar:4.3.4.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) 
```

环境:

1.  Spring Boot 1.4.2 版本
2.  Tomcat Embed 8.5.6

## 1.Spring Boot 环境

要编译 JSP，需要`tomcat-embed-jasper`。

pom.xml

```java
 <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.4.2.RELEASE</version>
	</parent>

	<dependencies>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>

		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
		</dependency>

	</dependencies> 
```

显示项目相关性:

```java
 $ mvn dependency:tree

[INFO] +- org.springframework.boot:spring-boot-starter-tomcat:jar:1.4.2.RELEASE:provided
[INFO] |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:8.5.6:provided
[INFO] |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:8.5.6:provided
[INFO] |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:8.5.6:provided
[INFO] +- org.apache.tomcat.embed:tomcat-embed-jasper:jar:8.5.6:provided
[INFO] |  \- org.eclipse.jdt.core.compiler:ecj:jar:4.5.1:provided
[INFO] \- javax.servlet:jstl:jar:1.2:compile 
```

 ## 2.解决办法

不知道为什么`tomcat-embed-jasper`没有使用 ecj 编译器，为了解决这个问题，Eclipse `ecj`手动声明:

pom.xml

```java
 <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.4.2.RELEASE</version>
	</parent>

	<dependencies>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>

		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
		</dependency>

		<!-- Add this -->
		<dependency>
			<groupId>org.eclipse.jdt.core.compiler</groupId>
			<artifactId>ecj</artifactId>
			<version>4.6.1</version>
			<scope>provided</scope>
		</dependency>

	</dependencies> 
```

显示项目相关性:

```java
 $ mvn dependency:tree

[INFO] +- org.springframework.boot:spring-boot-starter-tomcat:jar:1.4.2.RELEASE:provided
[INFO] |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:8.5.6:provided
[INFO] |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:8.5.6:provided
[INFO] |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:8.5.6:provided
[INFO] +- org.apache.tomcat.embed:tomcat-embed-jasper:jar:8.5.6:provided
[INFO] +- javax.servlet:jstl:jar:1.2:compile
[INFO] \- org.eclipse.jdt.core.compiler:ecj:jar:4.6.1:provided 
```

 ## 参考

1.  [使用嵌入式 Tomcat 创建一个 Java Web 应用程序](http://web.archive.org/web/20190303102708/https://devcenter.heroku.com/articles/create-a-java-web-application-using-embedded-tomcat)
2.  [Jasper 2 JSP 引擎如何](http://web.archive.org/web/20190303102708/https://tomcat.apache.org/tomcat-7.0-doc/jasper-howto.html)
3.  [可怕的没有可用的 Java 编译器](http://web.archive.org/web/20190303102708/http://tomcat.10.x6.nabble.com/Dreaded-No-Java-compiler-available-td1990531.html)







