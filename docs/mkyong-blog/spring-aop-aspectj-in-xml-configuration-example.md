# XML 配置示例中的 Spring AOP + AspectJ

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-aop-aspectj-in-xml-configuration-example/>

在本教程中，我们将向您展示如何将最后的 [Spring AOP + AspectJ 注释](http://web.archive.org/web/20190224164145/http://www.mkyong.com/spring3/spring-aop-aspectj-annotation-example/)转换成基于 XML 的配置。

对于那些不喜欢注释或使用 JDK 1.4 的人，可以使用基于 XML 的 AspectJ。

再次回顾上一个 customerBo 接口，用几个方法，稍后你将学习如何通过 AspectJ 在 XML 文件中截取它。

```java
 package com.mkyong.customer.bo;

public interface CustomerBo {

	void addCustomer();

	String addCustomerReturnValue();

	void addCustomerThrowException() throws Exception;

	void addCustomerAround(String name);
} 
```

## 1.AspectJ<before>= @之前</before>

AspectJ @Before 示例。

```java
 package com.mkyong.aspect;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect
public class LoggingAspect {

	@Before("execution(* com.mkyong.customer.bo.CustomerBo.addCustomer(..))")
	public void logBefore(JoinPoint joinPoint) {
		//...
	}

} 
```

XML 中的等效功能，用 **< aop:在>** 之前。

```java
 <!-- Aspect -->
<bean id="logAspect" class="com.mkyong.aspect.LoggingAspect" />

<aop:config>

  <aop:aspect id="aspectLoggging" ref="logAspect" >

     <!-- @Before -->
     <aop:pointcut id="pointCutBefore"
	expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomer(..))" />

     <aop:before method="logBefore" pointcut-ref="pointCutBefore" />

  </aop:aspect>

</aop:config> 
```

 ## 2.AspectJ <after>= @After</after>

AspectJ @After 示例。

```java
 package com.mkyong.aspect;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.After;

@Aspect
public class LoggingAspect {

	@After("execution(* com.mkyong.customer.bo.CustomerBo.addCustomer(..))")
	public void logAfter(JoinPoint joinPoint) {
		//...
	}

} 
```

XML 中的等效功能，用**<:AOP:在>** 之后。

```java
 <!-- Aspect -->
<bean id="logAspect" class="com.mkyong.aspect.LoggingAspect" />

<aop:config>

  <aop:aspect id="aspectLoggging" ref="logAspect" >

     <!-- @After -->
     <aop:pointcut id="pointCutAfter"
	expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomer(..))" />

     <aop:after method="logAfter" pointcut-ref="pointCutAfter" />

  </aop:aspect>

</aop:config> 
```

 ## 3.AspectJ <after-returning>= @AfterReturning</after-returning>

AspectJ @AfterReturning 示例。

```java
 package com.mkyong.aspect;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.AfterReturning;

@Aspect
public class LoggingAspect {

  @AfterReturning(
   pointcut = "execution(* com.mkyong.customer.bo.CustomerBo.addCustomerReturnValue(..))",
   returning= "result")
   public void logAfterReturning(JoinPoint joinPoint, Object result) {
	//...
   }

} 
```

XML 中的等效功能，用 **< aop:返回后>** 。

```java
 <!-- Aspect -->
<bean id="logAspect" class="com.mkyong.aspect.LoggingAspect" />

<aop:config>

  <aop:aspect id="aspectLoggging" ref="logAspect" >

    <!-- @AfterReturning -->
    <aop:pointcut id="pointCutAfterReturning"
      expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomerReturnValue(..))" />

    <aop:after-returning method="logAfterReturning" returning="result" 
      pointcut-ref="pointCutAfterReturning" />

  </aop:aspect>

</aop:config> 
```

## 4.AspectJ <after-throwing>= @AfterReturning</after-throwing>

AspectJ @AfterReturning 示例。

```java
 package com.mkyong.aspect;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.AfterThrowing;

@Aspect
public class LoggingAspect {

  @AfterThrowing(
   pointcut = "execution(* com.mkyong.customer.bo.CustomerBo.addCustomerThrowException(..))",
   throwing= "error")
  public void logAfterThrowing(JoinPoint joinPoint, Throwable error) {
	//...
  }
} 
```

XML 中的等效功能，用 **< aop:抛出后>** 。

```java
 <!-- Aspect -->
<bean id="logAspect" class="com.mkyong.aspect.LoggingAspect" />

<aop:config>

  <aop:aspect id="aspectLoggging" ref="logAspect" >

    <!-- @AfterThrowing -->
    <aop:pointcut id="pointCutAfterThrowing"
      expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomerThrowException(..))" />

    <aop:after-throwing method="logAfterThrowing" throwing="error" 
      pointcut-ref="pointCutAfterThrowing"  />

  </aop:aspect>

</aop:config> 
```

## 5.AspectJ<after-around>= @左右</after-around>

AspectJ @Around 示例。

```java
 package com.mkyong.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Around;

@Aspect
public class LoggingAspect {

	@Around("execution(* com.mkyong.customer.bo.CustomerBo.addCustomerAround(..))")
	public void logAround(ProceedingJoinPoint joinPoint) throws Throwable {
		//...
	}

} 
```

XML 中的等效功能，用 **< aop:after-around >** 。

```java
 <!-- Aspect -->
<bean id="logAspect" class="com.mkyong.aspect.LoggingAspect" />

<aop:config>

   <aop:aspect id="aspectLoggging" ref="logAspect" >

    <!-- @Around -->
   <aop:pointcut id="pointCutAround"
      expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomerAround(..))" />

   <aop:around method="logAround" pointcut-ref="pointCutAround"  />

  </aop:aspect>

</aop:config> 
```

## 完整的 XML 示例

参见完整的基于 AspectJ XML 的配置文件。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd 
	http://www.springframework.org/schema/aop 
	http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

<aop:aspectj-autoproxy />

<bean id="customerBo" class="com.mkyong.customer.bo.impl.CustomerBoImpl" />

<!-- Aspect -->
<bean id="logAspect" class="com.mkyong.aspect.LoggingAspect" />

<aop:config>

  <aop:aspect id="aspectLoggging" ref="logAspect">

    <!-- @Before -->
    <aop:pointcut id="pointCutBefore"
      expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomer(..))" />

    <aop:before method="logBefore" pointcut-ref="pointCutBefore" />

    <!-- @After -->
    <aop:pointcut id="pointCutAfter"
       expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomer(..))" />

    <aop:after method="logAfter" pointcut-ref="pointCutAfter" />

    <!-- @AfterReturning -->
    <aop:pointcut id="pointCutAfterReturning"
       expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomerReturnValue(..))" />

    <aop:after-returning method="logAfterReturning"
      returning="result" pointcut-ref="pointCutAfterReturning" />

    <!-- @AfterThrowing -->
    <aop:pointcut id="pointCutAfterThrowing"
      expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomerThrowException(..))" />

    <aop:after-throwing method="logAfterThrowing"
      throwing="error" pointcut-ref="pointCutAfterThrowing" />

    <!-- @Around -->
    <aop:pointcut id="pointCutAround"
      expression="execution(* com.mkyong.customer.bo.CustomerBo.addCustomerAround(..))" />

    <aop:around method="logAround" pointcut-ref="pointCutAround" />

  </aop:aspect>

</aop:config>

</beans> 
```

## 下载源代码

Download it – [Spring3-AOP-AspectJ-XML-Example.zip](http://web.archive.org/web/20190224164145/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-AOP-AspectJ-XML-Example.zip) (8 KB)

## 参考

1.  [Spring AOP + AspectJ 注释示例](http://web.archive.org/web/20190224164145/http://www.mkyong.com/spring3/spring-aop-aspectj-annotation-example/)
2.  [AspectJ 编程指南](http://web.archive.org/web/20190224164145/http://www.eclipse.org/aspectj/doc/released/progguide/index.html)
3.  [Spring AOP + AspectJ 引用](http://web.archive.org/web/20190224164145/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/aop.html)

[aop](http://web.archive.org/web/20190224164145/http://www.mkyong.com/tag/aop/) [aspectj](http://web.archive.org/web/20190224164145/http://www.mkyong.com/tag/aspectj/) [spring](http://web.archive.org/web/20190224164145/http://www.mkyong.com/tag/spring/)







