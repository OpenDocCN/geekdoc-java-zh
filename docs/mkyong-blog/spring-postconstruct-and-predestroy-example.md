# Spring @PostConstruct 和@PreDestroy 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-postconstruct-and-predestroy-example/>

在 Spring 中，可以实现 [InitializingBean 和 DisposableBean](http://web.archive.org/web/20190309050208/http://www.mkyong.com/spring/spring-initializingbean-and-disposablebean-example/) 接口，也可以在 Bean 配置文件中指定 [init-method 和 destroy-method](http://web.archive.org/web/20190309050208/http://www.mkyong.com/spring/spring-init-method-and-destroy-method-example/) 用于初始化和销毁回调函数。在本文中，我们将向您展示如何使用注释 **@PostConstruct** 和 **@PreDestroy** 来做同样的事情。

**Note**
The **@PostConstruct** and **@PreDestroy** annotation are not belong to Spring, it’s located in the J2ee library – common-annotations.jar.

## @PostConstruct 和@PreDestroy

带有@PostConstruct 和@PreDestroy 批注的 CustomerService bean

```java
 package com.mkyong.customer.services;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

public class CustomerService
{
	String message;

	public String getMessage() {
	  return message;
	}

	public void setMessage(String message) {
	  this.message = message;
	}

	@PostConstruct
	public void initIt() throws Exception {
	  System.out.println("Init method after properties are set : " + message);
	}

	@PreDestroy
	public void cleanUp() throws Exception {
	  System.out.println("Spring Container is destroy! Customer clean up");
	}

} 
```

默认情况下，Spring 不知道@PostConstruct 和@PreDestroy 注释。要启用它，您必须注册'**commannotationbeanpostprocessor**，或者在 bean 配置文件中指定'**<context:annotation-config/>**'。

 ## 1.commannotationbeanpostprocessor

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean class="org.springframework.context.annotation.CommonAnnotationBeanPostProcessor" />

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService">
		<property name="message" value="i'm property message" />
	</bean>

</beans> 
```

 ## 2.

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-2.5.xsd">

	<context:annotation-config />

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService">
		<property name="message" value="i'm property message" />
	</bean>

</beans> 
```

运行它

```java
 package com.mkyong.common;

import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.mkyong.customer.services.CustomerService;

public class App 
{
    public static void main( String[] args )
    {
    	ConfigurableApplicationContext context = 
    	  new ClassPathXmlApplicationContext(new String[] {"Spring-Customer.xml"});

    	CustomerService cust = (CustomerService)context.getBean("customerService");

    	System.out.println(cust);

    	context.close();
    }
} 
```

输出

```java
 Init method after properties are set : im property message
com.mkyong.customer.services.CustomerService@47393f
...
INFO: Destroying singletons in org.springframework.beans.factory.
support.DefaultListableBeanFactory@77158a: 
defining beans [customerService]; root of factory hierarchy
Spring Container is destroy! Customer clean up 
```

设置消息属性后，调用 **initIt()方法(@PostConstruct)** ，在 context.close()后调用 **cleanUp()方法(@ PreDestroy)**；

## 下载源代码

Download It – [Spring-PostConstruct-PreDestroy-Example.zip](http://web.archive.org/web/20190309050208/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-PostConstruct-PreDestroy-Example.zip)[spring](http://web.archive.org/web/20190309050208/http://www.mkyong.com/tag/spring/)







