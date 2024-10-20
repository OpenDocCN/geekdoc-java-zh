# Spring EL hello world 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-el-hello-world-example/>

Spring EL 类似于 OGNL 和 JSF EL，在 bean 创建期间被评估或执行。此外，所有 Spring 表达式都可以通过 XML 或注释获得。

在本教程中，我们将向您展示如何使用**Spring Expression Language(SpEL)**，在 XML 和注释中向属性中注入字符串、整数和 bean。

## 1.弹簧 EL 依赖关系

在 Maven `pom.xml`文件中声明核心 Spring jars，它将自动下载 Spring EL 依赖项。

*文件:pom.xml*

```java
 <properties>
		<spring.version>3.0.5.RELEASE</spring.version>
	</properties>

	<dependencies>

		<!-- Spring 3 dependencies -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>

	<dependencies> 
```

 ## 2.春豆

两个简单的 beans，后来使用 SpEL 在 XML 和注释中将值注入到属性中。

```java
 package com.mkyong.core;

public class Customer {

	private Item item;

	private String itemName;

} 
```

```java
 package com.mkyong.core;

public class Item {

	private String name;

	private int qty;

} 
```

 ## 3.XML 中的 Spring EL

SpEL 用`#{ SpEL expression }`括起来，参见 XML bean 定义文件中的以下示例。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="itemBean" class="com.mkyong.core.Item">
		<property name="name" value="itemA" />
		<property name="qty" value="10" />
	</bean>

	<bean id="customerBean" class="com.mkyong.core.Customer">
		<property name="item" value="#{itemBean}" />
		<property name="itemName" value="#{itemBean.name}" />
	</bean>

</beans> 
```

1.  **# { itemBean }**–将“item bean”注入到“customer bean”bean 的“item”属性中。
2.  **# { itemBean . name }**–将“item bean”的“name”属性注入到“customer bean”bean 的“itemName”属性中。

## 4.注释中的弹簧 EL

请在注释模式下查看等效版本。

**Note**
To use SpEL in annotation, you must register your component via annotation. If you register your bean in XML and define `@Value` in Java class, the `@Value` will failed to execute.

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("customerBean")
public class Customer {

	@Value("#{itemBean}")
	private Item item;

	@Value("#{itemBean.name}")
	private String itemName;

	//...

} 
```

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("itemBean")
public class Item {

	@Value("itemA") //inject String directly
	private String name;

	@Value("10") //inject interger directly
	private int qty;

	public String getName() {
		return name;
	}

	//...
} 
```

启用自动组件扫描。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.0.xsd">

	<context:component-scan base-package="com.mkyong.core" />

</beans> 
```

在注释模式下，使用`@Value`定义弹簧 EL。在这种情况下，您将一个字符串和整数值直接注入到" **itemBean** "中，然后将" itemBean "注入到" **customerBean** 属性中。

## 5.输出

运行它，XML 中的 SpEL 和注释都显示相同的结果:

```java
 package com.mkyong.core;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
	public static void main(String[] args) {
	    ApplicationContext context = new ClassPathXmlApplicationContext("SpringBeans.xml");

	    Customer obj = (Customer) context.getBean("customerBean");
	    System.out.println(obj);
	}
} 
```

*输出*

```java
 Customer [item=Item [name=itemA, qty=10], itemName=itemA] 
```

## 下载源代码

Download It – [Spring3-EL-Hello-Worldr-Example.zip](http://web.archive.org/web/20190219020010/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-EL-Hello-Worldr-Example.zip) (6 KB)

## 参考

1.  [弹簧 EL 参考值](http://web.archive.org/web/20190219020010/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/expressions.html)

[hello world](http://web.archive.org/web/20190219020010/http://www.mkyong.com/tag/hello-world/) [spring el](http://web.archive.org/web/20190219020010/http://www.mkyong.com/tag/spring-el/) [spring3](http://web.archive.org/web/20190219020010/http://www.mkyong.com/tag/spring3/)







