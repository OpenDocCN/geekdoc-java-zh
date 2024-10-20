# 自动扫描中的弹簧过滤器组件

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-filtering-components-in-auto-scanning/>

在本 [Spring 自动组件扫描教程](http://web.archive.org/web/20190816013001/http://www.mkyong.com/spring/spring-auto-scanning-components/)中，您将了解如何让 Spring 自动扫描您的组件。在这篇文章中，我们向您展示了如何在自动扫描过程中进行组件过滤。

## 1.过滤器组件–包括

参见下面的例子，使用 Spring " **filtering** 来扫描和注册与定义的" regex "匹配的组件名，即使该类没有用@Component 注释。

道层

```java
 package com.mkyong.customer.dao;

public class CustomerDAO 
{
	@Override
	public String toString() {
		return "Hello , This is CustomerDAO";
	}	
} 
```

服务层

```java
 package com.mkyong.customer.services;

import org.springframework.beans.factory.annotation.Autowired;
import com.mkyong.customer.dao.CustomerDAO;

public class CustomerService 
{
	@Autowired
	CustomerDAO customerDAO;

	@Override
	public String toString() {
		return "CustomerService [customerDAO=" + customerDAO + "]";
	}

} 
```

弹簧过滤。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-2.5.xsd">

	<context:component-scan base-package="com.mkyong" >

		<context:include-filter type="regex" 
                       expression="com.mkyong.customer.dao.*DAO.*" />

		<context:include-filter type="regex" 
                       expression="com.mkyong.customer.services.*Service.*" />

	</context:component-scan>

</beans> 
```

运行它

```java
 package com.mkyong.common;

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
    	System.out.println(cust);

    }
} 
```

输出

```java
 CustomerService [customerDAO=Hello , This is CustomerDAO] 
```

在这个 XML 过滤中，所有文件的名称都包含 DAO 或 Service (*DAO。*、*服务。*) word 将被检测并注册到 Spring 容器中。

## 2.过滤器组件–排除

另一方面，你也可以排除指定的组件，以避免 Spring 检测并在 Spring 容器中注册它。

排除那些用@Service 注释的文件。

```java
 <context:component-scan base-package="com.mkyong.customer" >
		<context:exclude-filter type="annotation" 
			expression="org.springframework.stereotype.Service" />		
	</context:component-scan> 
```

排除那些文件名中含有刀字的文件。

```java
 <context:component-scan base-package="com.mkyong" >
		<context:exclude-filter type="regex" 
			expression="com.mkyong.customer.dao.*DAO.*" />		
	</context:component-scan> 
```

## 下载源代码

Download It – [Spring-Filter-Auto-Scan-Example.zip](http://web.archive.org/web/20190816013001/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Filter-Auto-Scan-Example.zip)[spring](http://web.archive.org/web/20190816013001/https://www.mkyong.com/tag/spring/)<input type="hidden" id="mkyong-postId" value="3865">







