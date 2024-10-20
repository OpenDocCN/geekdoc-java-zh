# Spring AOP 示例——切入点、顾问

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-aop-example-pointcut-advisor/>

在最后一个 [Spring AOP 建议示例](http://web.archive.org/web/20210110080705/http://www.mkyong.com/spring/spring-aop-examples-advice/)中，一个类的所有方法都被自动拦截。但是在大多数情况下，你可能只需要一种方法来拦截一两个方法，这就是“切入点”的作用。它允许您通过方法名来截取方法。此外,“切入点”必须与“顾问”相关联。

在 Spring AOP 中，有三个非常专业的术语——**advice，切入点，Advisor** ，用非官方的方式来说…

*   建议–指明在方法执行之前或之后要采取的行动。
*   切入点——通过方法名或正则表达式模式指示应该截取哪个方法。
*   advisor——将“建议”和“切入点”组合成一个单元，并将其传递给代理工厂对象。

再次回顾上一个 [Spring AOP 建议示例](http://web.archive.org/web/20210110080705/http://www.mkyong.com/spring/spring-aop-examples-advice/)。

*文件:CustomerService.java*

```java
 package com.mkyong.customer.services;

public class CustomerService
{
	private String name;
	private String url;

	public void setName(String name) {
		this.name = name;
	}

	public void setUrl(String url) {
		this.url = url;
	}

	public void printName(){
		System.out.println("Customer name : " + this.name);
	}

	public void printURL(){
		System.out.println("Customer website : " + this.url);
	}

	public void printThrowException(){
		throw new IllegalArgumentException();
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
		<property name="name" value="Yong Mook Kim" />
		<property name="url" value="http://www.mkyong.com" />
	</bean>

	<bean id="hijackAroundMethodBeanAdvice" class="com.mkyong.aop.HijackAroundMethod" />

	<bean id="customerServiceProxy" 
                class="org.springframework.aop.framework.ProxyFactoryBean">

		<property name="target" ref="customerService" />

		<property name="interceptorNames">
			<list>
				<value>hijackAroundMethodBeanAdvice</value>
			</list>
		</property>
	</bean>
</beans> 
```

*文件:HijackAroundMethod.java*

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

		System.out.println("HijackAroundMethod : Before method hijacked!");

		try {
			Object result = methodInvocation.proceed();
			System.out.println("HijackAroundMethod : Before after hijacked!");

			return result;

		} catch (IllegalArgumentException e) {

			System.out.println("HijackAroundMethod : Throw exception hijacked!");
			throw e;
		}
	}
} 
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

		CustomerService cust = (CustomerService) appContext
				.getBean("customerServiceProxy");

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

客户服务类的整个方法都被拦截。稍后，我们将向您展示如何使用“**切入点**来拦截 only `printName()`方法。

## 切入点示例

您可以通过以下两种方式匹配方法:

1.  名称匹配
2.  常规压制比赛

## 1.切入点——名称匹配示例

通过“切入点”和“顾问”截取 printName()方法。创建一个 **NameMatchMethodPointcut** 切入点 bean，并将您想要截取的方法名放入“ **mappedName** 属性值中。

```java
 <bean id="customerPointcut"
        class="org.springframework.aop.support.NameMatchMethodPointcut">
		<property name="mappedName" value="printName" />
	</bean> 
```

创建一个**DefaultPointcutAdvisor**advisor bean，并关联通知和切入点。

```java
 <bean id="customerAdvisor"
		class="org.springframework.aop.support.DefaultPointcutAdvisor">
		<property name="pointcut" ref="customerPointcut" />
		<property name="advice" ref="hijackAroundMethodBeanAdvice" />
	</bean> 
```

将代理的“interceptorNames”替换为“customerAdvisor”(它是“hijackAroundMethodBeanAdvice”)。

```java
 <bean id="customerServiceProxy"
		class="org.springframework.aop.framework.ProxyFactoryBean">

		<property name="target" ref="customerService" />

		<property name="interceptorNames">
			<list>
				<value>customerAdvisor</value>
			</list>
		</property>
	</bean> 
```

完整的 bean 配置文件

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService">
		<property name="name" value="Yong Mook Kim" />
		<property name="url" value="http://www.mkyong.com" />
	</bean>

	<bean id="hijackAroundMethodBeanAdvice" class="com.mkyong.aop.HijackAroundMethod" />

	<bean id="customerServiceProxy" 
                class="org.springframework.aop.framework.ProxyFactoryBean">

		<property name="target" ref="customerService" />

		<property name="interceptorNames">
			<list>
				<value>customerAdvisor</value>
			</list>
		</property>
	</bean>

	<bean id="customerPointcut" 
                class="org.springframework.aop.support.NameMatchMethodPointcut">
		<property name="mappedName" value="printName" />
	</bean>

	<bean id="customerAdvisor" 
                 class="org.springframework.aop.support.DefaultPointcutAdvisor">
		<property name="pointcut" ref="customerPointcut" />
		<property name="advice" ref="hijackAroundMethodBeanAdvice" />
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
Customer website : http://www.mkyong.com
************************* 
```

现在，您只拦截 printName()方法。

**PointcutAdvisor**
Spring comes with **PointcutAdvisor** class to save your work to declare advisor and pointcut into different beans, you can use **NameMatchMethodPointcutAdvisor** to combine both into a single bean.

```java
 <bean id="customerAdvisor"
		class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor">

		<property name="mappedName" value="printName" />
		<property name="advice" ref="hijackAroundMethodBeanAdvice" />

	</bean> 
```

## 2.切入点–正则表达式示例

您还可以通过使用正则表达式 pointcut–**RegexpMethodPointcutAdvisor**来匹配方法的名称。

```java
 <bean id="customerAdvisor"
		class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
		<property name="patterns">
			<list>
				<value>.*URL.*</value>
			</list>
		</property>

		<property name="advice" ref="hijackAroundMethodBeanAdvice" />
	</bean> 
```

现在，它拦截方法名称中包含单词“URL”的方法。在实践中，你可以用它来管理 DAO 层，在这里你可以声明“**”。*道。*** "拦截你所有的 DAO 类来支持事务。

## 下载源代码

Download it – [Spring-AOP-Pointcuts-Advisor-Example.zip](http://web.archive.org/web/20210110080705/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-AOP-Pointcuts-Advisor-Example.zip)Tags : [aop](http://web.archive.org/web/20210110080705/https://mkyong.com/tag/aop/) [spring](http://web.archive.org/web/20210110080705/https://mkyong.com/tag/spring/)<input type="hidden" id="mkyong-current-postId" value="4012">

### 相关文章

*   [Spring AOP 拦截器事务不工作](/web/20210110080705/https://www.mkyong.com/spring/spring-aop-interceptor-transaction-is-not-working/)
*   [Spring AOP 错误:无法代理目标类，因为](/web/20210110080705/https://www.mkyong.com/spring/spring-aop-error-cannot-proxy-target-class-because-cglib2-is-not-available/)
*   [Spring AOP 示例-建议](/web/20210110080705/https://www.mkyong.com/spring/spring-aop-examples-advice/)
*   [Spring AOP 在 Hibernate 中的事务管理](/web/20210110080705/https://www.mkyong.com/spring/spring-aop-transaction-management-in-hibernate/)
*   [Spring AOP + AspectJ 注释示例](/web/20210110080705/https://www.mkyong.com/spring3/spring-aop-aspectj-annotation-example/)

*   [Spring AOP + AspectJ 在 XML 中的配置示例](/web/20210110080705/https://www.mkyong.com/spring3/spring-aop-aspectj-in-xml-configuration-example/)
*   [Wicket + Spring 集成示例](/web/20210110080705/https://www.mkyong.com/wicket/wicket-spring-integration-example/)
*   [Spring Autowiring @Qualifier 示例](/web/20210110080705/https://www.mkyong.com/spring/spring-autowiring-qualifier-example/)
*   [Maven + Spring hello world 示例](/web/20210110080705/https://www.mkyong.com/spring/quick-start-maven-spring-example/)
*   [如何加载多个弹簧豆配置文件](/web/20210110080705/https://www.mkyong.com/spring/load-multiple-spring-bean-configuration-file/)