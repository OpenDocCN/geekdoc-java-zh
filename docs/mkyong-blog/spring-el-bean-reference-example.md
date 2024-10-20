# Spring EL bean 参考示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-el-bean-reference-example/>

在 Spring EL 中，您可以引用一个 bean，并使用'**点(.)**符号。比如“ **bean.property_name** ”。

```java
 public class Customer {

	@Value("#{addressBean.country}")
	private String country; 
```

在上面的代码片段中，它将" **addressBean** " bean "中的" **country** 属性的值注入到当前的" **customer** 类，" **country** 属性中。

## 注释中的弹簧 EL

看下面的例子，向你展示如何使用 SpEL 来引用一个 bean，bean 属性和它的方法。

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("customerBean")
public class Customer {

	@Value("#{addressBean}")
	private Address address;

	@Value("#{addressBean.country}")
	private String country;

	@Value("#{addressBean.getFullAddress('mkyong')}")
	private String fullAddress;

	//getter and setter methods

	@Override
	public String toString() {
		return "Customer [address=" + address + "\n, country=" + country
				+ "\n, fullAddress=" + fullAddress + "]";
	}

} 
```

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("addressBean")
public class Address {

	@Value("Block ABC, LakeView")
	private String street;

	@Value("98700")
	private int postcode;

	@Value("US")
	private String country;

	public String getFullAddress(String prefix) {

		return prefix + " : " + street + " " + postcode + " " + country;
	}

	//getter and setter methods

	public void setCountry(String country) {
		this.country = country;
	}

	@Override
	public String toString() {
		return "Address [street=" + street + ", postcode=" + postcode
				+ ", country=" + country + "]";
	}

} 
```

*运行它*

```java
 Customer obj = (Customer) context.getBean("customerBean");
       System.out.println(obj); 
```

*输出*

```java
 Customer [address=Address [street=Block ABC, LakeView, postcode=98700, country=US]
, country=US
, fullAddress=mkyong : Block ABC, LakeView 98700 US] 
```

 ## XML 中的 Spring EL

请参见 bean 定义 XML 文件中的等效版本。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="customerBean" class="com.mkyong.core.Customer">
		<property name="address" value="#{addressBean}" />
		<property name="country" value="#{addressBean.country}" />
		<property name="fullAddress" value="#{addressBean.getFullAddress('mkyong')}" />
	</bean>

	<bean id="addressBean" class="com.mkyong.core.Address">
		<property name="street" value="Block ABC, LakeView" />
		<property name="postcode" value="98700" />
		<property name="country" value="US" />
	</bean>

</beans> 
```

 ## 下载源代码

Download It – [Spring3-EL-Bean-Reference-Example.zip](http://web.archive.org/web/20190302163512/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-EL-Bean-Reference-Example.zip) (6 KB)[spring el](http://web.archive.org/web/20190302163512/http://www.mkyong.com/tag/spring-el/) [spring3](http://web.archive.org/web/20190302163512/http://www.mkyong.com/tag/spring3/)







