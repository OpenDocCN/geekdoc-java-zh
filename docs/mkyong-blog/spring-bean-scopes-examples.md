# Spring bean 作用域示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-bean-scopes-examples/>

在 Spring 中，bean 作用域用于决定哪种类型的 bean 实例应该从 Spring 容器返回给调用者。

支持 5 种类型的 bean 作用域:

1.  singleton——为每个 Spring IoC 容器返回一个 bean 实例
2.  原型——每次请求时返回一个新的 bean 实例
3.  request——为每个 HTTP 请求返回一个 bean 实例。*
4.  session–为每个 HTTP 会话返回一个 bean 实例。*
5.  global session–为每个全局 HTTP 会话返回一个 bean 实例。*

在大多数情况下，您可能只处理 Spring 的核心范围——singleton 和 prototype，默认范围是 singleton。

*P.S *表示仅在 web 感知的 Spring 应用程序上下文的上下文中有效*

## 单体 vs 原型

这里有一个例子向您展示 bean 作用域之间的区别: **singleton** 和 **prototype** 。

```java
 package com.mkyong.customer.services;

public class CustomerService 
{
	String message;

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}
} 
```

## 1.单例示例

如果在 bean 配置文件中没有指定 bean 作用域，则默认为 singleton。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

       <bean id="customerService" 
            class="com.mkyong.customer.services.CustomerService" />

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
    	 new ClassPathXmlApplicationContext(new String[] {"Spring-Customer.xml"});

    	CustomerService custA = (CustomerService)context.getBean("customerService");
    	custA.setMessage("Message by custA");
    	System.out.println("Message : " + custA.getMessage());

    	//retrieve it again
    	CustomerService custB = (CustomerService)context.getBean("customerService");
    	System.out.println("Message : " + custB.getMessage());
    }
} 
```

输出

```java
 Message : Message by custA
Message : Message by custA 
```

因为 bean“customerService”在单例范围内，所以“custB”的第二次检索也将显示“custA”设置的消息，即使它是由新的 getBean()方法检索的。在 singleton 中，每个 Spring IoC 容器只有一个实例，不管用 getBean()检索多少次，它都将返回相同的实例。

## 2.原型示例

如果您想要一个新的“customerService”bean 实例，那么每次调用它时，请使用 prototype。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

   <bean id="customerService" class="com.mkyong.customer.services.CustomerService" 
         scope="prototype"/>

</beans> 
```

再运行一次

```java
 Message : Message by custA
Message : null 
```

在 prototype 范围内，每个被调用的`getBean()`方法都有一个新的实例。

## 3.Bean 范围注释

您还可以使用注释来定义您的 bean 范围。

```java
 package com.mkyong.customer.services;

import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Service;

@Service
@Scope("prototype")
public class CustomerService 
{
	String message;

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}
} 
```

启用自动组件扫描

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-2.5.xsd">

       <context:component-scan base-package="com.mkyong.customer" />

</beans> 
```

## 下载源代码

Download It – [Spring-Bean-Scopes-Example.zip](http://web.archive.org/web/20210506200112/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Bean-Scopes-Example.zip) (7 KB)

## 参考

1.  [http://static . springsource . org/spring/docs/2.0 . x/reference/beans . html # beans-factory-scopes](http://web.archive.org/web/20210506200112/http://static.springsource.org/spring/docs/2.0.x/reference/beans.html#beans-factory-scopes)

Tags : [spring](http://web.archive.org/web/20210506200112/https://mkyong.com/tag/spring/)<input type="hidden" id="mkyong-current-postId" value="3877">