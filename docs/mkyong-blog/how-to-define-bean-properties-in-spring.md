# 如何在 Spring 中为 bean 属性注入值

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/how-to-define-bean-properties-in-spring/>

在 Spring 中，有三种方法可以向 bean 属性注入值。

*   普通方法
*   捷径
*   “p”模式

请看一个简单的 Java 类，它包含两个属性——name 和 type。稍后，您将使用 Spring 向 bean 属性中注入值。

```java
 package com.mkyong.common;

public class FileNameGenerator 
{
	private String name;
	private String type;

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getType() {
		return type;
	}
	public void setType(String type) {
		this.type = type;
	}
} 
```

## 1.普通方法

在“value”标记内注入值，并用“property”标记括起来。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="FileNameGenerator" class="com.mkyong.common.FileNameGenerator">
		<property name="name">
			<value>mkyong</value>
		</property>
		<property name="type">
			<value>txt</value>
		</property>
	</bean>
</beans> 
```

 ## 2.捷径

用“值”属性注入值。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="FileNameGenerator" class="com.mkyong.common.FileNameGenerator">
		<property name="name" value="mkyong" />
		<property name="type" value="txt" />
	</bean>

</beans> 
```

 ## 3.“p”模式

通过使用“p”模式作为属性来注入值。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="FileNameGenerator" class="com.mkyong.common.FileNameGenerator" 
             p:name="mkyong" p:type="txt" />

</beans> 
```

记得在 Spring XML bean 配置文件中声明了**xmlns:p = " http://www . Spring framework . org/schema/p**。

## 结论

使用哪种方法完全取决于个人偏好，这不会影响注入到 bean 属性中的值。

[spring](http://web.archive.org/web/20190306103009/http://www.mkyong.com/tag/spring/)







