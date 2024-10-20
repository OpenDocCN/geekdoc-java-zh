# 弹簧 3 和 JSR-330 @注入和@命名示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring3/spring-3-and-jsr-330-inject-and-named-example/>

从 Spring 3.0 开始，Spring 支持标准的[JSR 330:Java 的依赖注入](http://web.archive.org/web/20221205171553/https://www.jcp.org/en/jsr/detail?id=330)。在 Spring 3 应用程序中，您可以使用标准

1.  `@Inject`代替春天的`@Autowired`来注射一颗豆子。
2.  `@Named`代替春天的`@Component`来宣告一颗豆子。

这些 JSR-330 标准注释的扫描和检索方式与 Spring 注释相同，集成是自动进行的，只要您的类路径中有以下 jar。

pom.xml

```java
 <dependency>
		<groupId>javax.inject</groupId>
		<artifactId>javax.inject</artifactId>
		<version>1</version>
	</dependency> 
```

## 1.弹簧注释

让我们看一个普通弹簧的注释例子—`@Autowired`和`@Component`

*PS`@Component`、`@Repository`和`@Service`相同，只是在 Spring Ioc 上下文中声明了一个 bean。*

CustomerDAO.java

```java
 package com.mkyong.customer.dao;

import org.springframework.stereotype.Repository;

@Repository
public class CustomerDAO 
{
	public void save() {
		System.out.println("CustomerDAO save method...");
	}	
} 
```

CustomerService.java

```java
 package com.mkyong.customer.services;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.mkyong.customer.dao.CustomerDAO;

@Service
public class CustomerService 
{
	@Autowired
	CustomerDAO customerDAO;

	public void save() {

		System.out.println("CustomerService save method...");
		customerDAO.save();

	}

} 
```

## 2.JSR-330 注解

基本上，它的工作原理是一样的，只是有不同的注释——`@Inject`和`@Named`。

CustomerDAO.java

```java
 package com.mkyong.customer.dao;

import javax.inject.Named;

@Named
public class CustomerDAO 
{

	public void save() {
		System.out.println("CustomerDAO save method...");
	}	
} 
```

CustomerService.java

```java
 package com.mkyong.customer.services;

import javax.inject.Inject;
import javax.inject.Named;

import com.mkyong.customer.dao.CustomerDAO;

@Named
public class CustomerService 
{
	@Inject
	CustomerDAO customerDAO;

	public void save() {

		System.out.println("CustomerService save method...");
		customerDAO.save();

	}

} 
```

## 3.运行它

Spring 和 JSR330 注释都需要组件扫描才能工作。

Spring-AutoScan.xml – Scan bean automatically

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context-3.1.xsd">

    <context:component-scan base-package="com.mkyong.customer" />

</beans> 
```

App.java – Run it

```java
 package com.mkyong;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.mkyong.customer.services.CustomerService;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
    		new ClassPathXmlApplicationContext(new String[] {"Spring-AutoScan.xml"});

    	CustomerService cust = (CustomerService)context.getBean("customerService");
    	cust.save();

    }
} 
```

以上两个例子都生成了相同的输出

```java
 CustomerService save method...
CustomerDAO save method... 
```

## 4.JSR-330 的局限性

与 Spring 相比，JSR-330 有一些局限性:

1.  `@Inject`没有确保 bean 成功注入所需的**属性。**
***   在 Spring 容器中，JSR-330 默认有作用域 singleton，但是你可以使用 Spring 的`@Scope`来定义其他。*   不等同于春天的`@Value`、`@Required`或`@Lazy`。**

 **检查这个[弹簧参考](http://web.archive.org/web/20221205171553/http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html/beans.html#beans-standard-annotations-limitations)。

## 5.去 JSR-330

其实 Spring 的注释更强大，只是在 Spring framework 上才有。JSR-330 是一个标准规范，遵循 JSR-330 规范的所有 J2ee 环境都支持它。

对于新项目或迁移项目，总是推荐使用 JSR-330 注释，并且记住，它也适用于 Spring 3。

## 下载源代码

Download it – [Spring-JSR330-Example.zip](http://web.archive.org/web/20221205171553/http://www.mkyong.com/wp-content/uploads/2012/09/Spring-JSR330-Example.zip) (27kb)

## 参考

1.  [弹簧参考:使用 JSR 330 标准注释](http://web.archive.org/web/20221205171553/http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html/beans.html#beans-standard-annotations)
2.  for Java 的依赖注入

<input type="hidden" id="mkyong-current-postId" value="12617">**