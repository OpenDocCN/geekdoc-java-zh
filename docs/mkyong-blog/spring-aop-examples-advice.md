# Spring AOP 示例——建议

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-aop-examples-advice/>

**Spring AOP + AspectJ**
Using AspectJ is more flexible and powerful, please refer to this tutorial – [Using AspectJ annotation in Spring AOP](http://web.archive.org/web/20190225095921/http://www.mkyong.com/spring3/spring-aop-aspectj-annotation-example/).

Spring AOP ( **面向方面编程**)框架用于模块化方面中的横切关注点。简单来说，它只是拦截某些进程的拦截器，比如当一个方法被执行时，Spring AOP 可以劫持正在执行的方法，并在方法执行之前或之后添加额外的功能。

在 Spring AOP 中，支持 4 种类型的建议:

*   建议前–在方法执行前运行
*   返回建议后–在方法返回结果后运行
*   抛出建议后–在方法抛出异常后运行
*   围绕建议——围绕方法执行，结合以上三个建议。

下面的例子向您展示了 Spring AOP 通知是如何工作的。

## 简单的弹簧示例

用几个打印方法创建一个简单的客户服务类，以便稍后演示。

```java
 package com.mkyong.customer.services;

public class CustomerService {
	private String name;
	private String url;

	public void setName(String name) {
		this.name = name;
	}

	public void setUrl(String url) {
		this.url = url;
	}

	public void printName() {
		System.out.println("Customer name : " + this.name);
	}

	public void printURL() {
		System.out.println("Customer website : " + this.url);
	}

	public void printThrowException() {
		throw new IllegalArgumentException();
	}

} 
```

文件:Spring-customer . XML–一个 bean 配置文件

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService">
		<property name="name" value="Yong Mook Kim" />
		<property name="url" value="http://www.mkyong.com" />
	</bean>

</beans> 
```

运行它

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.mkyong.customer.services.CustomerService;

public class App {
	public static void main(String[] args) {
		ApplicationContext appContext = new ClassPathXmlApplicationContext(
				new String[] { "Spring-Customer.xml" });

		CustomerService cust = (CustomerService) appContext.getBean("customerService");

		System.out.println("*************************");
		cust.printName();
		System.out.println("*************************");
		cust.printURL();
		System.out.println("*************************");
		try {
			cust.printThrowException();
		} catch (Exception e) {

		}

	}
} 
```

输出

```java
 *************************
Customer name : Yong Mook Kim
*************************
Customer website : http://www.mkyong.com
************************* 
```

一个简单的 Spring 项目到阿迪 bean 并输出一些字符串。

 ## 春季 AOP 建议

现在，将 Spring AOP 建议附加到上述客户服务中。

 ## 1.建议之前

它将在方法执行之前执行。创建一个实现 MethodBeforeAdvice 接口的类。

```java
 package com.mkyong.aop;

import java.lang.reflect.Method;
import org.springframework.aop.MethodBeforeAdvice;

public class HijackBeforeMethod implements MethodBeforeAdvice
{
	@Override
	public void before(Method method, Object[] args, Object target)
		throws Throwable {
	        System.out.println("HijackBeforeMethod : Before method hijacked!");
	}
} 
```

在 bean 配置文件(Spring-Customer.xml)中，为 **HijackBeforeMethod** 类创建一个 bean，并新建一个名为“ **customerServiceProxy** 的代理对象。

*   ‘target’–定义您想要劫持哪个 bean。
*   ‘interceptor names’–定义要在这个代理/目标对象上应用哪个类(通知)。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService">
		<property name="name" value="Yong Mook Kim" />
		<property name="url" value="http://www.mkyong.com" />
	</bean>

	<bean id="hijackBeforeMethodBean" class="com.mkyong.aop.HijackBeforeMethod" />

	<bean id="customerServiceProxy" 
                 class="org.springframework.aop.framework.ProxyFactoryBean">

		<property name="target" ref="customerService" />

		<property name="interceptorNames">
			<list>
				<value>hijackBeforeMethodBean</value>
			</list>
		</property>
	</bean>
</beans> 
```

**Note**
To use Spring proxy, you need to add CGLIB2 library. Add below in Maven pom.xml file.

```java
 <dependency>
		<groupId>cglib</groupId>
		<artifactId>cglib</artifactId>
		<version>2.2.2</version>
	</dependency> 
```

再次运行它，现在您得到了新的**customerServiceProxy**bean，而不是原来的 **customerService** bean。

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.mkyong.customer.services.CustomerService;

public class App {
	public static void main(String[] args) {
		ApplicationContext appContext = new ClassPathXmlApplicationContext(
				new String[] { "Spring-Customer.xml" });

		CustomerService cust = 
                                (CustomerService) appContext.getBean("customerServiceProxy");

		System.out.println("*************************");
		cust.printName();
		System.out.println("*************************");
		cust.printURL();
		System.out.println("*************************");
		try {
			cust.printThrowException();
		} catch (Exception e) {

		}

	}
} 
```

输出

```java
 *************************
HijackBeforeMethod : Before method hijacked!
Customer name : Yong Mook Kim
*************************
HijackBeforeMethod : Before method hijacked!
Customer website : http://www.mkyong.com
*************************
HijackBeforeMethod : Before method hijacked! 
```

在执行每个 customerService 的方法之前，它将运行 **HijackBeforeMethod 的 before()** 方法。

## 2.返回建议后

它将在方法返回结果后执行。创建一个在 ReturningAdvice 接口后实现**的类。**

```java
 package com.mkyong.aop;

import java.lang.reflect.Method;
import org.springframework.aop.AfterReturningAdvice;

public class HijackAfterMethod implements AfterReturningAdvice
{
	@Override
	public void afterReturning(Object returnValue, Method method,
		Object[] args, Object target) throws Throwable {
	        System.out.println("HijackAfterMethod : After method hijacked!");
	}
} 
```

Bean 配置文件

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService">
		<property name="name" value="Yong Mook Kim" />
		<property name="url" value="http://www.mkyong.com" />
	</bean>

	<bean id="hijackAfterMethodBean" class="com.mkyong.aop.HijackAfterMethod" />

	<bean id="customerServiceProxy" 
                class="org.springframework.aop.framework.ProxyFactoryBean">

		<property name="target" ref="customerService" />

		<property name="interceptorNames">
			<list>
				<value>hijackAfterMethodBean</value>
			</list>
		</property>
	</bean>
</beans> 
```

再运行一次，输出

```java
 *************************
Customer name : Yong Mook Kim
HijackAfterMethod : After method hijacked!
*************************
Customer website : http://www.mkyong.com
HijackAfterMethod : After method hijacked!
************************* 
```

在返回结果的每个 customerService 的方法之后，它将运行 **HijackAfterMethod 的 afterReturning()** 方法。

## 3.抛出建议后

它将在方法引发异常后执行。创建一个实现 ThrowsAdvice 接口的类，创建一个 **afterThrowing** 方法劫持 **IllegalArgumentException** 异常。

```java
 package com.mkyong.aop;

import org.springframework.aop.ThrowsAdvice;

public class HijackThrowException implements ThrowsAdvice {
	public void afterThrowing(IllegalArgumentException e) throws Throwable {
		System.out.println("HijackThrowException : Throw exception hijacked!");
	}
} 
```

Bean 配置文件

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService">
		<property name="name" value="Yong Mook Kim" />
		<property name="url" value="http://www.mkyong.com" />
	</bean>

	<bean id="hijackThrowExceptionBean" class="com.mkyong.aop.HijackThrowException" />

	<bean id="customerServiceProxy" 
                 class="org.springframework.aop.framework.ProxyFactoryBean">

		<property name="target" ref="customerService" />

		<property name="interceptorNames">
			<list>
				<value>hijackThrowExceptionBean</value>
			</list>
		</property>
	</bean>
</beans> 
```

再运行一次，输出

```java
 *************************
Customer name : Yong Mook Kim
*************************
Customer website : http://www.mkyong.com
*************************
HijackThrowException : Throw exception hijacked! 
```

如果 customerService 的方法抛出异常，它将运行 **HijackThrowException 的 afterThrowing()** 方法。

## 4.围绕建议

它结合了上述所有三个建议，并在方法执行期间执行。创建一个实现 **MethodInterceptor** 接口的类。你要调用**" method invocation . proceed()；**“继续原方法执行，否则原方法不会执行。

```java
 package com.mkyong.aop;

import java.util.Arrays;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;

public class HijackAroundMethod implements MethodInterceptor {
	@Override
	public Object invoke(MethodInvocation methodInvocation) throws Throwable {

		System.out.println("Method name : "
				+ methodInvocation.getMethod().getName());
		System.out.println("Method arguments : "
				+ Arrays.toString(methodInvocation.getArguments()));

		// same with MethodBeforeAdvice
		System.out.println("HijackAroundMethod : Before method hijacked!");

		try {
			// proceed to original method call
			Object result = methodInvocation.proceed();

			// same with AfterReturningAdvice
			System.out.println("HijackAroundMethod : Before after hijacked!");

			return result;

		} catch (IllegalArgumentException e) {
			// same with ThrowsAdvice
			System.out.println("HijackAroundMethod : Throw exception hijacked!");
			throw e;
		}
	}
} 
```

Bean 配置文件

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService">
		<property name="name" value="Yong Mook Kim" />
		<property name="url" value="http://www.mkyong.com" />
	</bean>

	<bean id="hijackAroundMethodBean" class="com.mkyong.aop.HijackAroundMethod" />

	<bean id="customerServiceProxy" 
                class="org.springframework.aop.framework.ProxyFactoryBean">

		<property name="target" ref="customerService" />

		<property name="interceptorNames">
			<list>
				<value>hijackAroundMethodBean</value>
			</list>
		</property>
	</bean>
</beans> 
```

再运行一次，输出

```java
 *************************
Method name : printName
Method arguments : []
HijackAroundMethod : Before method hijacked!
Customer name : Yong Mook Kim
HijackAroundMethod : Before after hijacked!
*************************
Method name : printURL
Method arguments : []
HijackAroundMethod : Before method hijacked!
Customer website : http://www.mkyong.com
HijackAroundMethod : Before after hijacked!
*************************
Method name : printThrowException
Method arguments : []
HijackAroundMethod : Before method hijacked!
HijackAroundMethod : Throw exception hijacked! 
```

在每个 customerService 的方法执行之后，它将运行 **HijackAroundMethod 的 invoke()** 方法。

## 结论

大多数 Spring 开发者只是实现了‘Around advice ’,因为它可以应用所有的建议类型，但是更好的实践应该选择最合适的建议类型来满足需求。

**Pointcut**
In this example, all the methods in a customer service class are intercepted (advice) automatically. But for most cases, you may need to use [Pointcut and Advisor](http://web.archive.org/web/20190225095921/http://www.mkyong.com/spring/spring-aop-example-pointcut-advisor/) to intercept a method via it’s method name.

## 下载源代码

Download it – [Spring-AOP-Advice-Examples.zip](http://web.archive.org/web/20190225095921/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-AOP-Advice-Examples.zip) (8 KB)[aop](http://web.archive.org/web/20190225095921/http://www.mkyong.com/tag/aop/) [spring](http://web.archive.org/web/20190225095921/http://www.mkyong.com/tag/spring/)







