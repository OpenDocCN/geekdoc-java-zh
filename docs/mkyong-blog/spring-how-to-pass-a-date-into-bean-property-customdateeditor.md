# Spring 将日期注入 bean 属性–custom Date editor

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-how-to-pass-a-date-into-bean-property-customdateeditor/>

这个 Spring 示例向您展示了如何将一个“日期”注入到 bean 属性中。

```java
 package com.mkyong.common;

import java.util.Date;

public class Customer {

	Date date;

	public Date getDate() {
		return date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

	@Override
	public String toString() {
		return "Customer [date=" + date + "]";
	}

} 
```

Bean 配置文件

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customer" class="com.mkyong.common.Customer">
		<property name="date" value="2010-01-31" />
	</bean>

</beans> 
```

运行它

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext(
				"SpringBeans.xml");

		Customer cust = (Customer) context.getBean("customer");
		System.out.println(cust);

	}
} 
```

您将遇到以下错误消息:

```java
 Caused by: org.springframework.beans.TypeMismatchException: 
	Failed to convert property value of type [java.lang.String] to 
	required type [java.util.Date] for property 'date'; 

nested exception is java.lang.IllegalArgumentException: 
	Cannot convert value of type [java.lang.String] to
	required type [java.util.Date] for property 'date': 
	no matching editors or conversion strategy found 
```

## 解决办法

在 Spring 中，您可以通过两种方法注入日期:

 ## 1.工厂豆

声明一个日期格式 bean，在“客户”bean 中，引用“日期格式”bean 作为工厂 bean。工厂方法会调用`SimpleDateFormat.parse()`自动将字符串转换成日期对象。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="dateFormat" class="java.text.SimpleDateFormat">
		<constructor-arg value="yyyy-MM-dd" />
	</bean>

	<bean id="customer" class="com.mkyong.common.Customer">
		<property name="date">
			<bean factory-bean="dateFormat" factory-method="parse">
				<constructor-arg value="2010-01-31" />
			</bean>
		</property>
	</bean>

</beans> 
```

 ## 2.自定义日期编辑器

声明一个 CustomDateEditor 类，将 String 转换成 **java.util.Date** 。

```java
 <bean id="dateEditor"
	   class="org.springframework.beans.propertyeditors.CustomDateEditor">

		<constructor-arg>
			<bean class="java.text.SimpleDateFormat">
				<constructor-arg value="yyyy-MM-dd" />
			</bean>
		</constructor-arg>
		<constructor-arg value="true" />
	</bean> 
```

并声明另一个“CustomEditorConfigurer”，使 Spring 转换类型为 **java.util.Date** 的 bean 属性。

```java
 <bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
		<property name="customEditors">
			<map>
				<entry key="java.util.Date">
					<ref local="dateEditor" />
				</entry>
			</map>
		</property>
	</bean> 
```

bean 配置文件的完整示例。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="dateEditor"
		class="org.springframework.beans.propertyeditors.CustomDateEditor">

		<constructor-arg>
			<bean class="java.text.SimpleDateFormat">
				<constructor-arg value="yyyy-MM-dd" />
			</bean>
		</constructor-arg>
		<constructor-arg value="true" />

	</bean>

	<bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
		<property name="customEditors">
			<map>
				<entry key="java.util.Date">
					<ref local="dateEditor" />
				</entry>
			</map>
		</property>
	</bean>

	<bean id="customer" class="com.mkyong.common.Customer">
		<property name="date" value="2010-02-31" />
	</bean>

</beans> 
```

## 下载源代码

Download It – [Spring-CustomDateEditor-Example.zip](http://web.archive.org/web/20190213135022/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-CustomDateEditor-Examples.zip) (5KB)

## 参考

1.  [春天房产编辑](http://web.archive.org/web/20190213135022/http://static.springsource.org/spring/docs/2.5.6/api/org/springframework/beans/propertyeditors/package-tree.html)

[date](http://web.archive.org/web/20190213135022/http://www.mkyong.com/tag/date/) [spring](http://web.archive.org/web/20190213135022/http://www.mkyong.com/tag/spring/)







