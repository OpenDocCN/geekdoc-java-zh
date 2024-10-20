# 在 Spring 中定义自定义@Required-style 注释

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-define-custom-required-style-annotation/>

[@Required 注释](http://web.archive.org/web/20190210020246/http://www.mkyong.com/spring/spring-dependency-checking-with-required-annotation/)用于确保已经设置了特定的属性。如果您正在将现有项目迁移到 Spring framework，或者出于某种原因拥有自己的@Required-style 注释，Spring 允许您定义自定义的@Required-style 注释，这相当于@Required 注释。

在这个例子中，您将创建一个名为 **@Mandatory** 的自定义`@Required-style`注释，它相当于`@Required`注释。

## 1.创建@Mandatory 接口

```java
 package com.mkyong.common;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Mandatory {
} 
```

 ## 2.将其应用于一个属性

```java
 package com.mkyong.common;

public class Customer 
{
	private Person person;
	private int type;
	private String action;

	@Mandatory
	public void setPerson(Person person) {
		this.person = person;
	}
	//getter and setter methods
} 
```

 ## 3.注册它

在“RequiredAnnotationBeanPostProcessor”类中包含新的 **@Mandatory** 批注。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

<bean 
class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor">
	<property name="requiredAnnotationType" value="com.mkyong.common.Mandatory"/>
</bean>

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<property name="action" value="buy" />
		<property name="type" value="1" />
	</bean>

</beans> 
```

## 4.完成的

完成后，您就创建了一个新的名为 **@Mandatory** 的自定义@Required-style 注释，它相当于@Required 注释。

[spring](http://web.archive.org/web/20190210020246/http://www.mkyong.com/tag/spring/)







