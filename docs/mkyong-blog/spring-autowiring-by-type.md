# 按类型排列的弹簧自动布线

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-autowiring-by-type/>

在 Spring 中，“**按类型**自动连接”意味着，如果一个 bean 的数据类型与其他 bean 属性的数据类型兼容，就自动连接它。

例如，一个“person”bean 公开了一个数据类型为“capability”类的属性，Spring 将找到数据类型为“capability”类的 bean，并自动连接它。如果没有找到匹配，就什么都不做。

您可以通过`autowire="byType"`启用该功能，如下所示:

```java
 <!-- person has a property type of class "ability" -->
	<bean id="person" class="com.mkyong.common.Person" autowire="byType" />

	<bean id="invisible" class="com.mkyong.common.Ability" >
		<property name="skill" value="Invisible" />
	</bean> 
```

按类型查看 Spring 自动布线的完整示例。

## 1.豆子

两颗豆子，人和能力。

```java
 package com.mkyong.common;

public class Person 
{
	private Ability ability;
	//...
} 
```

```java
 package com.mkyong.common;

public class Ability 
{
	private String skill;
	//...
} 
```

 ## 2.弹簧布线

通常，您显式连接 bean:

```java
 <bean id="person" class="com.mkyong.common.Person">
		<property name="ability" ref="invisible" />
	</bean>

	<bean id="invisible" class="com.mkyong.common.Ability" >
		<property name="skill" value="Invisible" />
	</bean> 
```

*输出*

```java
 Person [ability=Ability [skill=Invisible]] 
```

启用**自动按类型连线**后，您可以不设置能力属性。Spring 将找到相同的数据类型并自动连接它。

```java
 <bean id="person" class="com.mkyong.common.Person" autowire="byType" />

	<bean id="invisible" class="com.mkyong.common.Ability" >
		<property name="skill" value="Invisible" />
	</bean> 
```

*输出*

```java
 Person [ability=Ability [skill=Invisible]] 
```

等等，如果你有两个数据类型相同的类“能力”的 beans 呢？

```java
 <bean id="person" class="com.mkyong.common.Person" autowire="byType" />

	<bean id="steal" class="com.mkyong.common.Ability" >
		<property name="skill" value="Steal" />
	</bean>

	<bean id="invisible" class="com.mkyong.common.Ability" >
		<property name="skill" value="Invisible" />
	</bean> 
```

*输出*

```java
 Exception in thread "main" org.springframework.beans.factory.UnsatisfiedDependencyException: 
...
No unique bean of type [com.mkyong.common.Ability] is defined: 
expected single matching bean but found 2: [steal, invisible]; nested exception is 
org.springframework.beans.factory.NoSuchBeanDefinitionException: 
No unique bean of type [com.mkyong.common.Ability] is defined: 
expected single matching bean but found 2: [steal, invisible] 
```

在这种情况下，你会点击`UnsatisfiedDependencyException`错误信息。

**Note**
In autowiring by type mode, you just have to make sure only one unique data type of bean is declared. ## 下载源代码

Download it – [Spring-AutoWiring-by-Type-Example.zip](http://web.archive.org/web/20190302163344/http://www.mkyong.com/wp-content/uploads/2011/06/Spring-AutoWiring-by-Type-Example.zip) (6 KB)[spring](http://web.archive.org/web/20190302163344/http://www.mkyong.com/tag/spring/) [wiring](http://web.archive.org/web/20190302163344/http://www.mkyong.com/tag/wiring/)







