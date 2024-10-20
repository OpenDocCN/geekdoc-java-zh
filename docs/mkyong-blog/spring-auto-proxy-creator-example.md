# Spring 自动代理创建器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-auto-proxy-creator-example/>

在去年春天的 AOP 示例中——[advice](http://web.archive.org/web/20190225101459/http://www.mkyong.com/spring/spring-aop-examples-advice/)、 [pointcut 和 advisor](http://web.archive.org/web/20190225101459/http://www.mkyong.com/spring/spring-aop-example-pointcut-advisor/) ，您必须为每个需要 AOP 支持的 bean 手工创建一个代理 bean (ProxyFactoryBean)。

这不是一种有效的方式，例如，如果您希望 customer 模块中的所有 DAO 类都实现带有 SQL 日志支持(advise)的 AOP 特性，那么您必须手动创建许多代理工厂 bean，结果是您的 bean 配置文件中充斥了大量的代理 bean。

幸运的是，Spring 附带了两个自动代理创建器来自动为您的 beans 创建代理。

## 1.BeanNameAutoProxyCreator 示例

在此之前，您必须手动创建一个代理 bean (ProxyFactoryBean)。

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

	<bean id="customerAdvisor"
		class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor">
		<property name="mappedName" value="printName" />
		<property name="advice" ref="hijackAroundMethodBeanAdvice" />
	</bean>
</beans> 
```

并获取代理名为“customerServiceProxy”的 bean。

```java
 CustomerService cust = (CustomerService)appContext.getBean("customerServiceProxy"); 
```

在自动代理机制中，您只需要创建一个 **BeanNameAutoProxyCreator** ，并将您的所有 bean(通过 bean 名称或正则表达式名称)和“advisor”包含到一个单元中。

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

	<bean id="customerAdvisor"
		class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor">
		<property name="mappedName" value="printName" />
		<property name="advice" ref="hijackAroundMethodBeanAdvice" />
	</bean>

	<bean
		class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">
		<property name="beanNames">
			<list>
				<value>*Service</value>
			</list>
		</property>
		<property name="interceptorNames">
			<list>
				<value>customerAdvisor</value>
			</list>
		</property>
	</bean>
</beans> 
```

现在，您可以通过原始名称“customerService”获得 bean，您甚至不知道这个 bean 已经被代理。

```java
 CustomerService cust = (CustomerService)appContext.getBean("customerService"); 
```

 ## 2.DefaultAdvisorAutoProxyCreator 示例

这个**DefaultAdvisorAutoProxyCreator**非常强大，如果任何一个 beans 被 advisor 匹配，Spring 将自动为它创建一个代理。

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

	<bean id="customerAdvisor"
		class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor">
		<property name="mappedName" value="printName" />
		<property name="advice" ref="hijackAroundMethodBeanAdvice" />
	</bean>

       <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" />

</beans> 
```

这只是越权，因为你不能控制 bean 应该代理什么，你能做的只是相信 Spring 会为你做得最好。如果你想在你的项目中实现它，请小心。

 ## 下载源代码

Download it – [Spring-Auto-Proxy-Creator-Example.zip](http://web.archive.org/web/20190225101459/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Auto-Proxy-Creator-Example.zip)[proxy](http://web.archive.org/web/20190225101459/http://www.mkyong.com/tag/proxy/) [spring](http://web.archive.org/web/20190225101459/http://www.mkyong.com/tag/spring/)







