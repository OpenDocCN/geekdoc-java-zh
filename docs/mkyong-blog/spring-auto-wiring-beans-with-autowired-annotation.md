# 带有@Autowired 注释的 Spring 自动连接 Beans

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-auto-wiring-beans-with-autowired-annotation/>

在 XML 的最后一个 [Spring 自动连接示例中，它将自动连接当前 Spring 容器中任何 bean 的匹配属性。在大多数情况下，您可能只需要特定 bean 中的 autowired 属性。](http://web.archive.org/web/20210506141505/http://www.mkyong.com/spring/spring-auto-wiring-beans-in-xml/)

在 Spring 中，您可以使用 **@Autowired** 注释来自动连接 setter 方法、构造函数或字段上的 bean。此外，它可以自动连接特定 bean 中的属性。

**Note**
The @Autowired annotation is auto wire the bean by matching data type.

参见下面的完整示例，演示如何使用**@自动连线**。

## 1.豆子

一个客户 bean，并在 bean 配置文件中声明。稍后，您将使用“ **@Autowired** ”来自动连接一个人 bean。

```java
 package com.mkyong.common;

public class Customer 
{
	//you want autowired this field.
	private Person person;

	private int type;
	private String action;

	//getter and setter method

} 
```

```java
 <beans 
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<property name="action" value="buy" />
		<property name="type" value="1" />
	</bean>

	<bean id="PersonBean" class="com.mkyong.common.Person">
		<property name="name" value="mkyong" />
		<property name="address" value="address 123" />
		<property name="age" value="28" />
	</bean>

</beans> 
```

## 2.注册 AutowiredAnnotationBeanPostProcessor

要启用 **@Autowired** ，需要注册“**AutowiredAnnotationBeanPostProcessor**”，有两种方式:

##### 1.包括

在 bean 配置文件中添加 Spring 上下文和。

```java
 <beans 
	//...
	xmlns:context="http://www.springframework.org/schema/context"
	//...
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-2.5.xsd">
	//...

	<context:annotation-config />
	//...
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

##### 2.包括 AutowiredAnnotationBeanPostProcessor

将“AutowiredAnnotationBeanPostProcessor”直接包含在 bean 配置文件中。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

<bean 
class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/>

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

## 3.@自动连线示例

现在，您可以通过 **@Autowired** 自动连接 bean，它可以应用于 setter 方法、构造函数或字段。

##### 1.@Autowired setter 方法

```java
 package com.mkyong.common;

import org.springframework.beans.factory.annotation.Autowired;

public class Customer 
{
	private Person person;
	private int type;
	private String action;
	//getter and setter methods

	@Autowired
	public void setPerson(Person person) {
		this.person = person;
	}
} 
```

##### 2.@autowired 构造函数

```java
 package com.mkyong.common;

import org.springframework.beans.factory.annotation.Autowired;

public class Customer 
{
	private Person person;
	private int type;
	private String action;
	//getter and setter methods

	@Autowired
	public Customer(Person person) {
		this.person = person;
	}
} 
```

##### 3.@自动连线字段

```java
 package com.mkyong.common;

import org.springframework.beans.factory.annotation.Autowired;

public class Customer 
{
	@Autowired
	private Person person;
	private int type;
	private String action;
	//getter and setter methods
} 
```

上面的示例将“PersonBean”自动连接到客户的个人属性中。

*运行它*

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
    	  new ClassPathXmlApplicationContext(new String[] {"SpringBeans.xml"});

    	Customer cust = (Customer)context.getBean("CustomerBean");
    	System.out.println(cust);

    }
} 
```

*输出*

```java
 Customer [action=buy, type=1, 
person=Person [address=address 123, age=28, name=mkyong]] 
```

## 依赖性检查

默认情况下，@Autowired 将执行依赖项检查，以确保属性已正确连接。当 Spring 找不到匹配的 bean 来连接时，它会抛出一个异常。要解决这个问题，可以通过将@Autowired 的“ **required** ”属性设置为 false 来禁用这个检查特性。

```java
 package com.mkyong.common;

import org.springframework.beans.factory.annotation.Autowired;

public class Customer 
{
	@Autowired(required=false)
	private Person person;
	private int type;
	private String action;
	//getter and setter methods
} 
```

在上面的例子中，如果 Spring 找不到匹配的 bean，它将保留 person 属性不变。

## @限定符

@Qualifier 注释用于控制应该在字段上自动连接哪个 bean。例如，bean 配置文件包含两个相似的 person beans。

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

	<bean id="PersonBean1" class="com.mkyong.common.Person">
		<property name="name" value="mkyong1" />
		<property name="address" value="address 1" />
		<property name="age" value="28" />
	</bean>

	<bean id="PersonBean2" class="com.mkyong.common.Person">
		<property name="name" value="mkyong2" />
		<property name="address" value="address 2" />
		<property name="age" value="28" />
	</bean>

</beans> 
```

Spring 会知道应该给哪个 bean 布线吗？

要解决这个问题，您可以使用 **@Qualifier** 来自动连接特定的 bean，例如，

```java
 package com.mkyong.common;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class Customer 
{
	@Autowired
	@Qualifier("PersonBean1")
	private Person person;
	private int type;
	private String action;
	//getter and setter methods
} 
```

这意味着，bean“person bean 1”被自动绑定到客户的个人属性中。阅读完整示例—[Spring auto wiring @ Qualifier 示例](http://web.archive.org/web/20210506141505/http://www.mkyong.com/spring/spring-autowiring-qualifier-example/)

## 结论

这个 **@Autowired** 注释高度灵活且功能强大，绝对优于 bean 配置文件中的“ **autowire** ”属性。

## 下载源代码

Download It – [Spring-Autowired-Annotation-Example.zip](http://web.archive.org/web/20210506141505/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Autowired-Annotation-Example.zip) (6 KB)Tags : [spring](http://web.archive.org/web/20210506141505/https://mkyong.com/tag/spring/) [wiring](http://web.archive.org/web/20210506141505/https://mkyong.com/tag/wiring/)<input type="hidden" id="mkyong-current-postId" value="3712">