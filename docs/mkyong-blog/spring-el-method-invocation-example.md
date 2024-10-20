# Spring EL 方法调用示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-el-method-invocation-example/>

Spring expression language (SpEL)允许开发人员使用表达式来执行方法，并将方法返回值注入到属性中，即所谓的“ **SpEL 方法调用**”。

## 注释中的弹簧 EL

查看如何使用 **@Value** 注释进行 Spring EL 方法调用。

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("customerBean")
public class Customer {

	@Value("#{'mkyong'.toUpperCase()}")
	private String name;

	@Value("#{priceBean.getSpecialPrice()}")
	private double amount;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public double getAmount() {
		return amount;
	}

	public void setAmount(double amount) {
		this.amount = amount;
	}

	@Override
	public String toString() {
		return "Customer [name=" + name + ", amount=" + amount + "]";
	}

} 
```

```java
 package com.mkyong.core;

import org.springframework.stereotype.Component;

@Component("priceBean")
public class Price {

	public double getSpecialPrice() {
		return new Double(99.99);
	}

} 
```

*输出*

```java
 Customer [name=MKYONG, amount=99.99] 
```

##### 说明

在**字符串**文本上调用“`toUpperCase()`”方法。

```java
 @Value("#{'mkyong'.toUpperCase()}")
	private String name; 
```

对 bean ' **priceBean** '调用'`getSpecialPrice()`方法。

```java
 @Value("#{priceBean.getSpecialPrice()}")
	private double amount; 
```

 ## XML 中的 Spring EL

这是 bean 定义 XML 文件中的等效版本。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="customerBean" class="com.mkyong.core.Customer">
		<property name="name" value="#{'mkyong'.toUpperCase()}" />
		<property name="amount" value="#{priceBean.getSpecialPrice()}" />
	</bean>

	<bean id="priceBean" class="com.mkyong.core.Price" />

</beans> 
```

*输出*

```java
 Customer [name=MKYONG, amount=99.99] 
```

 ## 下载源代码

Download It – [Spring3-EL-Method-Invocation-Example.zip](http://web.archive.org/web/20190220135005/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-EL-Method-Invocation-Example.zip) (6 KB)[spring el](http://web.archive.org/web/20190220135005/http://www.mkyong.com/tag/spring-el/) [spring3](http://web.archive.org/web/20190220135005/http://www.mkyong.com/tag/spring3/)







