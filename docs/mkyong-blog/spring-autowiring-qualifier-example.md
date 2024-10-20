# Spring 自动布线@Qualifier 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-autowiring-qualifier-example/>

在 Spring 中，@Qualifier 表示哪个 bean 有资格在字段上自动绑定。请参见以下场景:

## 自动布线示例

参见下面的例子，它将一个“person”bean 自动连接到客户的 person 属性中。

```java
 package com.mkyong.common;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class Customer {

	@Autowired
	private Person person;
	//...
} 
```

但是，在 bean 配置文件中声明了两个相似的 bean“`com.mkyong.common.Person`”。Spring 会知道应该自动连线哪个人 bean 吗？

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

<bean 
class ="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/>

	<bean id="customer" class="com.mkyong.common.Customer" />

	<bean id="personA" class="com.mkyong.common.Person" >
		<property name="name" value="mkyongA" />
	</bean>

	<bean id="personB" class="com.mkyong.common.Person" >
		<property name="name" value="mkyongB" />
	</bean>

</beans> 
```

当您运行以上示例时，会遇到以下异常:

```java
 Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: 
	No unique bean of type [com.mkyong.common.Person] is defined: 
		expected single matching bean but found 2: [personA, personB] 
```

 ## @限定符示例

要解决上述问题，您需要 **@Quanlifier** 告诉 Spring 应该自动连接哪个 bean。

```java
 package com.mkyong.common;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class Customer {

	@Autowired
	@Qualifier("personA")
	private Person person;
	//...
} 
```

在这种情况下，bean“personA”是自动连接的。

```java
 Customer [person=Person [name=mkyongA]] 
```

 ## 下载源代码

Download It – [Spring-AutoWiring-Qualifier-Example.zip](http://web.archive.org/web/20190309053207/http://www.mkyong.com/wp-content/uploads/2011/06/Spring-AutoWiring-Qualifier-Example.zip) (6 KB)

## 参考

1.  [Spring @Autowired 示例](http://web.archive.org/web/20190309053207/http://www.mkyong.com/spring/spring-auto-wiring-beans-with-autowired-annotation/)

[spring](http://web.archive.org/web/20190309053207/http://www.mkyong.com/tag/spring/) [wiring](http://web.archive.org/web/20190309053207/http://www.mkyong.com/tag/wiring/)







