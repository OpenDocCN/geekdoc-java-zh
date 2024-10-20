# Spring 3 JavaConfig 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-3-javaconfig-example/>

由于 Spring 3， **JavaConfig** 特性包含在核心 Spring 模块中，它允许开发人员将 bean 定义和 Spring 配置从 XML 文件移到 Java 类中。

但是，您仍然可以使用经典的 XML 方式来定义 beans 和配置， **JavaConfig** 只是另一种替代解决方案。

查看经典 XML 定义和 JavaConfig 之间的区别，在 Spring 容器中定义 bean。

*Spring XML 文件:*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="helloBean" class="com.mkyong.hello.impl.HelloWorldImpl">

</beans> 
```

*Java config 中的等效配置:*

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.mkyong.hello.HelloWorld;
import com.mkyong.hello.impl.HelloWorldImpl;

@Configuration
public class AppConfig {

    @Bean(name="helloBean")
    public HelloWorld helloWorld() {
        return new HelloWorldImpl();
    }

} 
```

## Spring JavaConfig Hello World

现在，请看一个完整的 Spring JavaConfig 示例。

 ## 1.目录结构

请参见本示例的目录结构。

![directory structure of this example](img/af02bb5afa129a50134f3eacc741d7dc.png "spring-javaconfig-folder") ## 2.依赖库

要使用 Java config(**@ Configuration**)，需要包含 **CGLIB** 库。查看依赖关系:

```java
 <!-- Spring 3 dependencies -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
		<version>${spring.version}</version>
	</dependency>

	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>${spring.version}</version>
	</dependency>

	<!-- JavaConfig need this library -->
	<dependency>
		<groupId>cglib</groupId>
		<artifactId>cglib</artifactId>
		<version>2.2.2</version>
	</dependency> 
```

## 3.春豆

一颗简单的豆子。

```java
 package com.mkyong.hello;

public interface HelloWorld {

	void printHelloWorld(String msg);

} 
```

```java
 package com.mkyong.hello.impl;

import com.mkyong.hello.HelloWorld;

public class HelloWorldImpl implements HelloWorld {

	@Override
	public void printHelloWorld(String msg) {

		System.out.println("Hello : " + msg);
	}

} 
```

## 4.JavaConfig 注释

用`@Configuration`注释告诉 Spring 这是核心的 Spring 配置文件，并通过`@Bean`定义 bean。

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.mkyong.hello.HelloWorld;
import com.mkyong.hello.impl.HelloWorldImpl;

@Configuration
public class AppConfig {

    @Bean(name="helloBean")
    public HelloWorld helloWorld() {
        return new HelloWorldImpl();
    }

} 
```

## 5.运行它

用`AnnotationConfigApplicationContext`加载您的 JavaConfig 类。

```java
 package com.mkyong.core;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import com.mkyong.config.AppConfig;
import com.mkyong.hello.HelloWorld;

public class App {
	public static void main(String[] args) {

            ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
	    HelloWorld obj = (HelloWorld) context.getBean("helloBean");

	    obj.printHelloWorld("Spring3 Java Config");

	}
} 
```

*输出*

```java
 Hello : Spring3 Java Config 
```

## 下载源代码

Download It – [Spring3-JavaConfig-Example.zip](http://web.archive.org/web/20190304170607/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-JavaConfig-Example.zip) (6 KB)

## 参考

1.  [Spring 3 JavaConfig 引用](http://web.archive.org/web/20190304170607/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/new-in-3.html#new-feature-java-config)

[javaconfig](http://web.archive.org/web/20190304170607/http://www.mkyong.com/tag/javaconfig/) [spring3](http://web.archive.org/web/20190304170607/http://www.mkyong.com/tag/spring3/)







