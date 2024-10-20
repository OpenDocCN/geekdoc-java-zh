# Spring InitializingBean 和 DisposableBean 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-initializingbean-and-disposablebean-example/>

在 Spring 中， **InitializingBean** 和 **DisposableBean** 是两个标记接口，这是 Spring 在 Bean 初始化和销毁时执行某些操作的有用方法。

1.  对于 Bean 实现的 InitializingBean，它将在设置完所有 bean 属性后运行`afterPropertiesSet()`。
2.  对于 Bean 实现的 DisposableBean，它将在 Spring 容器释放 bean 后运行`destroy()`。

## 例子

这里有一个例子向你展示如何使用 **InitializingBean** 和 **DisposableBean** 。实现 InitializingBean 和 DisposableBean 接口的 CustomerService bean，并具有消息属性。

```java
 package com.mkyong.customer.services;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class CustomerService implements InitializingBean, DisposableBean
{
	String message;

	public String getMessage() {
	  return message;
	}

	public void setMessage(String message) {
	  this.message = message;
	}

	public void afterPropertiesSet() throws Exception {
	  System.out.println("Init method after properties are set : " + message);
	}

	public void destroy() throws Exception {
	  System.out.println("Spring Container is destroy! Customer clean up");
	}

} 
```

*文件:Spring-Customer.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

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

**configurableapplicationcontext . close()**将关闭应用程序上下文，释放所有资源并销毁所有缓存的单体 beans。它仅用于`destroy()`方法演示:)

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

设置消息属性后，调用 afterPropertiesSet()方法。而 destroy()方法是在 context.close()之后调用的；

**Thoughts…**
I would not recommend to use InitializingBean and DisposableBean interface, because it will tight coupled your code to Spring. A better approach should be specifying the [init-method and destroy-method](http://web.archive.org/web/20190310101221/http://www.mkyong.com/spring/spring-init-method-and-destroy-method-example/) attributes in your bean configuration file. ## 下载源代码

Download It – [Spring-InitializingBean-DisposableBean-Example.zip](http://web.archive.org/web/20190310101221/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-InitializingBean-DisposableBean-Example.zip) ## 参考

1.  [初始化 Bean Javadoc](http://web.archive.org/web/20190310101221/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/beans/factory/InitializingBean.html)
2.  [一次性 Bean Javadoc](http://web.archive.org/web/20190310101221/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/beans/factory/DisposableBean.html)

[spring](http://web.archive.org/web/20190310101221/http://www.mkyong.com/tag/spring/)







