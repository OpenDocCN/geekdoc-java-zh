# 弹簧按名称自动布线

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-autowiring-by-name/>

在 Spring 中，“**按名称**自动连接”意味着，如果一个 bean 的名称与其他 bean 属性的名称相同，就自动连接它。

例如，如果一个“客户”bean 公开了一个“地址”属性，Spring 将在当前容器中找到“地址”bean 并自动连接它。如果没有找到匹配，就什么都不做。

您可以通过`autowire="byName"`启用该功能，如下所示:

```java
 <!-- customer has a property name "address" -->
	<bean id="customer" class="com.mkyong.common.Customer" autowire="byName" />

	<bean id="address" class="com.mkyong.common.Address" >
		<property name="fulladdress" value="Block A 888, CA" />
	</bean> 
```

按名称查看 Spring 自动布线的完整示例。

## 1.豆子

两个 beans，客户和地址。

```java
 package com.mkyong.common;

public class Customer 
{
	private Address address;
	//...
} 
```

```java
 package com.mkyong.common;

public class Address 
{
	private String fulladdress;
	//...
} 
```

 ## 2.弹簧布线

通常，通过 ref 属性显式连接 bean，如下所示:

```java
 <bean id="customer" class="com.mkyong.common.Customer" >
		<property name="address" ref="address" />
	</bean>

	<bean id="address" class="com.mkyong.common.Address" >
		<property name="fulladdress" value="Block A 888, CA" />
	</bean> 
```

*输出*

```java
 Customer [address=Address [fulladdress=Block A 888, CA]] 
```

启用了**auto wire by name**后，您不再需要声明属性标签。只要“地址”bean 与“客户”bean 的属性“地址”同名，Spring 就会自动连接它。

```java
 <bean id="customer" class="com.mkyong.common.Customer" autowire="byName" />

	<bean id="address" class="com.mkyong.common.Address" >
		<property name="fulladdress" value="Block A 888, CA" />
	</bean> 
```

*输出*

```java
 Customer [address=Address [fulladdress=Block A 888, CA]] 
```

再看另一个例子，这一次，连线会失败，导致 bean“address ABC”与 bean“customer”的属性名不匹配。

```java
 <bean id="customer" class="com.mkyong.common.Customer" autowire="byName" />

	<bean id="addressABC" class="com.mkyong.common.Address" >
		<property name="fulladdress" value="Block A 888, CA" />
	</bean> 
```

*输出*

```java
 Customer [address=null] 
```

 ## 下载源代码

Download it – [Spring-AutoWiring-by-Name-Example.zip](http://web.archive.org/web/20190310093451/http://www.mkyong.com/wp-content/uploads/2011/06/Spring-AutoWiring-by-Name-Example.zip) (6 KB)[spring](http://web.archive.org/web/20190310093451/http://www.mkyong.com/tag/spring/) [wiring](http://web.archive.org/web/20190310093451/http://www.mkyong.com/tag/wiring/)







