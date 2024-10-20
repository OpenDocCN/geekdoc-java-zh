# 带有@Required 注释的 Spring 依赖检查

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-dependency-checking-with-required-annotation/>

Spring 在 bean 配置文件中的[依赖检查](http://web.archive.org/web/20220316201912/http://www.mkyong.com/spring/spring-properties-dependency-checking/)用于确保某个类型(原语、集合或对象)的所有属性都已设置。在大多数情况下，您只需要确保设置了特定的属性，而不是所有的属性..

对于这种情况，您需要 **@Required** 注释，参见以下示例:

## @必需示例

一个客户对象，在 setPerson()方法中应用@Required 以确保已经设置了 Person 属性。

```java
 package com.mkyong.common;

import org.springframework.beans.factory.annotation.Required;

public class Customer 
{
	private Person person;
	private int type;
	private String action;

	public Person getPerson() {
		return person;
	}
	@Required
	public void setPerson(Person person) {
		this.person = person;
	}
} 
```

简单地应用@Required 注释将不会强制执行属性检查，您还需要注册一个**RequiredAnnotationBeanPostProcessor**来了解 bean 配置文件中的@Required 注释。

可以通过两种方式启用 RequiredAnnotationBeanPostProcessor。

##### 1.包括

在 bean 配置文件中添加 Spring 上下文和。

```java
 <beans 
	...
	xmlns:context="http://www.springframework.org/schema/context"
	...
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-2.5.xsd">
	...
	<context:annotation-config />
	...
</beans> 
```

完整示例，

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-2.5.xsd">

	<context:annotation-config />

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<property name="action" value="buy" />
		<property name="type" value="1" />
	</bean>

	<bean id="PersonBean" class="com.mkyong.common.Person">
		<property name="name" value="mkyong" />
		<property name="address" value="address ABC" />
		<property name="age" value="29" />
	</bean>

</beans> 
```

##### 2.包含 RequiredAnnotationBeanPostProcessor

在 bean 配置文件中直接包含“RequiredAnnotationBeanPostProcessor”。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

<bean 
class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor"/>

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<property name="action" value="buy" />
		<property name="type" value="1" />
	</bean>

	<bean id="PersonBean" class="com.mkyong.common.Person">
		<property name="name" value="mkyong" />
		<property name="address" value="address ABC" />
		<property name="age" value="29" />
	</bean>

</beans> 
```

如果运行它，将会抛出下面的错误消息，因为 person 属性未设置。

```java
 org.springframework.beans.factory.BeanInitializationException: 
	Property 'person' is required for bean 'CustomerBean' 
```

## 结论

尝试@Required 批注，它比 XML 文件中的依赖检查更灵活，因为它只能应用于特定的属性。

**Custom @Required**
Please read this article about [how to create a new custom @Required-style annotation](http://web.archive.org/web/20220316201912/http://www.mkyong.com/spring/spring-define-custom-required-style-annotation/).

## 参考

1.  [http://static . springsource . org/spring/docs/2.5 . x/reference/metadata . html # metadata-annotations-required](http://web.archive.org/web/20220316201912/http://static.springsource.org/spring/docs/2.5.x/reference/metadata.html#metadata-annotations-required)

<input type="hidden" id="mkyong-current-postId" value="3677">