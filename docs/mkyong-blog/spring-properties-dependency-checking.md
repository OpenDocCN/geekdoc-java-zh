# Spring 属性依赖检查

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-properties-dependency-checking/>

在 Spring 中，您可以使用依赖检查特性来确保已经设置或注入了所需的属性。

## 依赖性检查模式

支持 4 种依赖检查模式:

*   none–不进行依赖性检查。
*   简单——如果原始类型(int，long，double…)和集合类型(map，list..)尚未设置，将引发 UnsatisfiedDependencyException。
*   objects–如果尚未设置对象类型的任何属性，将引发 UnsatisfiedDependencyException。
*   all–如果尚未设置任何类型的任何属性，将抛出一个 UnsatisfiedDependencyException
    。

*P.S 默认模式为无*

 ## 例子

演示的客户和人员对象。

```java
 package com.mkyong.common;

public class Customer 
{
	private Person person;
	private int type;
	private String action;

	//getter and setter methods
} 
```

```java
 package com.mkyong.common;

public class Person 
{
	private String name;
	private String address;
	private int age;

	//getter and setter methods	
} 
```

 ## 1.无依赖性检查

具有“无”依赖性检查模式的 Spring bean 配置文件。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer" >
		<property name="action" value="buy" />
	</bean>

	<bean id="PersonBean" class="com.mkyong.common.Person">
		<property name="name" value="mkyong" />
		<property name="address" value="address ABC" />
		<property name="age" value="29" />
	</bean>

</beans> 
```

如果没有明确定义依赖检查模式，则默认为“无”。将不执行任何依赖性检查。

## 2.简单的依赖性检查

具有“简单”依赖检查模式的 Spring bean 配置文件。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer" 
         dependency-check="simple">

		<property name="person" ref="PersonBean" />
		<property name="action" value="buy" />
	</bean>

	<bean id="PersonBean" class="com.mkyong.common.Person">
		<property name="name" value="mkyong" />
		<property name="address" value="address ABC" />
		<property name="age" value="29" />
	</bean>

</beans> 
```

尚未设置“类型”属性(基本类型或集合类型)，将引发一个**UnsatisfiedDependencyException**。

```java
 org.springframework.beans.factory.UnsatisfiedDependencyException: 
Error creating bean with name 'CustomerBean' 
defined in class path resource [config/Spring-Customer.xml]: 
Unsatisfied dependency expressed through bean property 'type': 
Set this property value or disable dependency checking for this bean. 
```

## 3.对象依赖性检查

具有“对象”依赖性检查模式的 Spring bean 配置文件。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer" 
         dependency-check="objects">

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

“人员”属性(对象类型)尚未设置，将引发一个**UnsatisfiedDependencyException**。

```java
 org.springframework.beans.factory.UnsatisfiedDependencyException: 
Error creating bean with name 'CustomerBean' 
defined in class path resource [config/Spring-Customer.xml]: 
Unsatisfied dependency expressed through bean property 'person': 
Set this property value or disable dependency checking for this bean. 
```

## 4.所有依赖性检查

具有“全部”依赖性检查模式的 Spring bean 配置文件。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer" 
         dependency-check="all">

		<property name="action" value="buy" />
	</bean>

	<bean id="PersonBean" class="com.mkyong.common.Person">
		<property name="name" value="mkyong" />
		<property name="address" value="address ABC" />
		<property name="age" value="29" />
	</bean>

</beans> 
```

“简单”和“对象”模式的组合，如果任何类型(原语、集合和对象)的任何属性尚未设置，将会抛出一个**UnsatisfiedDependencyException**。

## 全局默认依赖性检查

为每个 bean 显式定义依赖关系检查模式是繁琐且容易出错的，您可以在<beans>根元素中设置一个 default-dependency-check 属性，以强制在<beans>根元素中声明的所有 bean 应用此规则。但是，这个根默认模式将被 bean 自己的模式覆盖(如果指定的话)。</beans></beans>

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd" 
	default-dependency-check="all">

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

此配置文件中声明的所有 beans 都默认为“All”依赖项检查模式。

**@Required Annotation**
In most scenarios, you just need to make sure a particular property has been set, but not all properties of a certain types (primitive, collection or object). The @Required Annotation can enforce this checking, [see detail](http://web.archive.org/web/20190225104756/http://www.mkyong.com/spring/spring-dependency-checking-with-required-annotation/).[spring](http://web.archive.org/web/20190225104756/http://www.mkyong.com/tag/spring/)







