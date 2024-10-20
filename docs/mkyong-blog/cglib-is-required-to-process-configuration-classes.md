# 处理@Configuration 类需要 CGLIB

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/cglib-is-required-to-process-configuration-classes/>

## 问题

使用 Spring3 `@Configuration`创建如下所示的应用程序配置文件:

```java
 import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

	@Bean
   //...

} 
```

但是，当运行它时，它会显示以下错误消息:

```java
 org.springframework.context.support.AbstractApplicationContext prepareRefresh
//...
Exception in thread "main" java.lang.IllegalStateException: 
CGLIB is required to process @Configuration classes. 
Either add CGLIB to the classpath or remove the following 
@Configuration bean definitions: [appConfig]
//...
at com.mkyong.core.App.main(App.java:12) 
```

## 解决办法

要在 Spring 3 中使用`@Configuration`，需要手动包含 **CGLIB** 库，只需在 Maven `pom.xml`文件中声明即可。

```java
 <dependency>
		<groupId>cglib</groupId>
		<artifactId>cglib</artifactId>
		<version>2.2.2</version>
	</dependency> 
```

Tags : [cglib](http://web.archive.org/web/20200616173123/https://mkyong.com/tag/cglib/) [spring3](http://web.archive.org/web/20200616173123/https://mkyong.com/tag/spring3/)<input type="hidden" id="mkyong-current-postId" value="9324">