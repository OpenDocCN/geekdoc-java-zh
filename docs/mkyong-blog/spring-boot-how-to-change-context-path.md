# Spring Boot——如何改变语境路径

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-how-to-change-context-path/>

在 Spring Boot，要改变上下文路径，更新`server.contextPath`属性。以下示例将上下文路径从`/`更新为`/mkyong`或`http://localhost:8080/mkyong`

**Note**
By default, the context path is “/”.

*PS 用 Spring Boot 1.4.2.RELEASE 测试*

## 1.属性和 Yaml

1.1 通过属性文件更新。

/src/main/resources/application.properties

```java
 server.port=8080
server.contextPath=/mkyong 
```

1.2 通过 yaml 文件更新。

/src/main/resources/application.yml

```java
 server:
  port: 8080
  contextPath: /mkyong 
```

## 2.嵌入式 ServletContainerCustomizer

通过代码更新，这将覆盖属性和 yaml 设置。

CustomContainer.java

```java
 package com.mkyong;

import org.springframework.boot.context.embedded.ConfigurableEmbeddedServletContainer;
import org.springframework.boot.context.embedded.EmbeddedServletContainerCustomizer;
import org.springframework.stereotype.Component;

@Component
public class CustomContainer implements EmbeddedServletContainerCustomizer {

	@Override
	public void customize(ConfigurableEmbeddedServletContainer container) {

		container.setPort(8080);
		container.setContextPath("/mkyong");

	}

} 
```

## 3.命令行

通过直接传递系统属性来更新上下文路径。

Terminal

```java
 java -jar -Dserver.contextPath=/mkyong spring-boot-example-1.0.jar 
```

## 参考

1.  [Spring Boot——嵌入式 servlet 容器](http://web.archive.org/web/20220815221144/https://docs.spring.io/spring-boot/docs/current/reference/html/howto-embedded-servlet-containers.html)
2.  [Spring Boot–外部化配置](http://web.archive.org/web/20220815221144/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)
3.  [Spring Boot——如何更改 Tomcat 端口](http://web.archive.org/web/20220815221144/http://www.mkyong.com/spring-boot/spring-boot-how-to-change-tomcat-port/)

<input type="hidden" id="mkyong-current-postId" value="14223">