# Spring EL 正则表达式示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-el-regular-expression-example/>

Spring EL 支持正则表达式使用一个简单的关键字“ **matches** ”，真的很牛逼！举个例子，

```java
 @Value("#{'100' matches '\\d+' }")
	private boolean isDigit; 
```

它通过正则表达式' **\\d+** '测试' **100** 是否为有效数字。

## 注释中的弹簧 EL

请看下面的 Spring EL 正则表达式示例，其中一些混合了三元运算符，这使得 Spring EL 非常灵活和强大。

下面的例子应该是不言自明的。

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("customerBean")
public class Customer {

	// email regular expression
	String emailRegEx = "^[_A-Za-z0-9-]+(\\.[_A-Za-z0-9-]+)" +
			"*@[A-Za-z0-9]+(\\.[A-Za-z0-9]+)*(\\.[A-Za-z]{2,})$";

	// if this is a digit?
	@Value("#{'100' matches '\\d+' }")
	private boolean validDigit;

	// if this is a digit + ternary operator
	@Value("#{ ('100' matches '\\d+') == true ? " +
			"'yes this is digit' : 'No this is not a digit'  }")
	private String msg;

	// if this emailBean.emailAddress contains a valid email address?
	@Value("#{emailBean.emailAddress matches customerBean.emailRegEx}")
	private boolean validEmail;

	//getter and setter methods, and constructor	
} 
```

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("emailBean")
public class Email {

	@Value("nospam@abc.com")
	String emailAddress;

	//...
} 
```

*输出*

```java
 Customer [isDigit=true, msg=yes this is digit, isValidEmail=true] 
```

 ## XML 中的 Spring EL

请参见 bean 定义 XML 文件中的等效版本。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="customerBean" class="com.mkyong.core.Customer">
	  <property name="validDigit" value="#{'100' matches '\d+' }" />
	  <property name="msg"
		value="#{ ('100' matches '\d+') == true ? 'yes this is digit' : 'No this is not a digit'  }" />
	  <property name="validEmail"
		value="#{emailBean.emailAddress matches '^[_A-Za-z0-9-]+(\.[_A-Za-z0-9-]+)*@[A-Za-z0-9]+(\.[A-Za-z0-9]+)*(\.[A-Za-z]{2,})/ins> }" />
	</bean>

	<bean id="emailBean" class="com.mkyong.core.Email">
	  <property name="emailAddress" value="nospam@abc.com" />
	</bean>

</beans> 
```

 ## 下载源代码

Download It – [Spring3-EL-Regular-Expression-Example.zip](http://web.archive.org/web/20190225103233/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-EL-Regular-Expression-Example.zip) (6 KB)

## 参考

1.  [电子邮件正则表达式示例](http://web.archive.org/web/20190225103233/http://www.mkyong.com/regular-expressions/how-to-validate-email-address-with-regular-expression/)
2.  [Spring EL 三元运算符(if-then-else)示例](http://web.archive.org/web/20190225103233/http://www.mkyong.com/spring3/spring-el-ternary-operator-if-then-else-example/)

[regex](http://web.archive.org/web/20190225103233/http://www.mkyong.com/tag/regex/) [spring el](http://web.archive.org/web/20190225103233/http://www.mkyong.com/tag/spring-el/) [spring3](http://web.archive.org/web/20190225103233/http://www.mkyong.com/tag/spring3/)







