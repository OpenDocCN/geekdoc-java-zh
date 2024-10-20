# Spring 自动布线 Beans

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-auto-wiring-beans-in-xml/>

在 Spring framework 中，您可以使用自动连接特性自动连接 beans。要启用它，只需在< bean >中定义“**自动连线**属性。

```java
 <bean id="customer" class="com.mkyong.common.Customer" autowire="byName" /> 
```

在 Spring 中，支持 5 种自动布线模式。

*   否–默认，无自动布线，通过“ref”属性手动设置
*   按名称–按属性名称自动布线。如果一个 bean 的名称与其他 bean 属性的名称相同，则自动连接它。
*   byType–按属性数据类型自动连接。如果一个 bean 的数据类型与其他 bean 属性的数据类型兼容，则自动连接它。
*   constructor–构造函数参数中的 byType 模式。
*   自动检测–如果找到默认的构造函数，使用“由构造函数自动连接”；否则，使用“按类型自动关联”。

## 例子

用于自动布线演示的客户和人员对象。

```java
 package com.mkyong.common;

public class Customer 
{
	private Person person;

	public Customer(Person person) {
		this.person = person;
	}

	public void setPerson(Person person) {
		this.person = person;
	}
	//...
} 
```

```java
 package com.mkyong.common;

public class Person 
{
	//...
} 
```

 ## 1.自动布线'否'

这是默认模式，您需要通过“ref”属性连接您的 bean。

```java
 <bean id="customer" class="com.mkyong.common.Customer">
                  <property name="person" ref="person" />
	</bean>

	<bean id="person" class="com.mkyong.common.Person" /> 
```

 ## 2.自动布线'按名称'

通过属性名自动连接 bean。在这种情况下，由于“person”bean 的名称与“customer”bean 的属性名称(“person”)相同，因此，Spring 将通过 setter 方法自动连接它—“`setPerson(Person person)`”。

```java
 <bean id="customer" class="com.mkyong.common.Customer" autowire="byName" />

	<bean id="person" class="com.mkyong.common.Person" /> 
```

参见完整示例-[名为](http://web.archive.org/web/20190223073503/http://www.mkyong.com/spring/spring-autowiring-by-name/)的弹簧自动布线。

## 3.自动布线'按类型'

通过属性数据类型自动连接 bean。在这种情况下，由于“person”bean 的数据类型与“customer”bean 的属性(Person 对象)的数据类型相同，所以 Spring 将通过 setter 方法自动连接它。

```java
 <bean id="customer" class="com.mkyong.common.Customer" autowire="byType" />

	<bean id="person" class="com.mkyong.common.Person" /> 
```

参见完整示例-[按类型](http://web.archive.org/web/20190223073503/http://www.mkyong.com/spring/spring-autowiring-by-type/)进行弹簧自动布线。

## 4.自动布线'构造器'

通过构造函数参数中的属性数据类型自动连接 bean。在这种情况下，由于“person”bean 的数据类型与“customer”bean 的属性(Person 对象)中的构造函数参数数据类型相同，因此，Spring 通过构造函数方法——“`public Customer(Person person)`”自动连接它。

```java
 <bean id="customer" class="com.mkyong.common.Customer" autowire="constructor" />

	<bean id="person" class="com.mkyong.common.Person" /> 
```

参见完整示例-[由构造者](http://web.archive.org/web/20190223073503/http://www.mkyong.com/spring/spring-autowiring-by-constructor/)进行弹簧自动布线。

## 5.自动布线'自动检测'

如果找到默认构造函数，则使用“构造函数”；否则，使用“byType”。在这种情况下，由于“Customer”类中有一个默认的构造函数，所以 Spring 通过构造函数方法-“`public Customer(Person person)`”自动连接它。

```java
 <bean id="customer" class="com.mkyong.common.Customer" autowire="autodetect" />

	<bean id="person" class="com.mkyong.common.Person" /> 
```

参见完整示例-[通过自动检测](http://web.archive.org/web/20190223073503/http://www.mkyong.com/spring/spring-autowiring-by-autodetect/)触发自动布线。

**Note**
It’s always good to combine both ‘auto-wire’ and ‘dependency-check’ together, to make sure the property is always auto-wire successfully.

```java
 <bean id="customer" class="com.mkyong.common.Customer" 
			autowire="autodetect" dependency-check="objects />

	<bean id="person" class="com.mkyong.common.Person" /> 
```

## 结论

在我看来，Spring 的“自动连接”以巨大的代价加快了开发速度——它增加了整个 bean 配置文件的复杂性，你甚至不知道哪个 bean 将自动连接到哪个 bean 中。

在实践中，我宁愿手动连线，它总是很干净，工作很完美，或者更好地使用 [@Autowired annotation](http://web.archive.org/web/20190223073503/http://www.mkyong.com/spring/spring-auto-wiring-beans-with-autowired-annotation/) ，这更灵活，也更值得推荐。

[spring](http://web.archive.org/web/20190223073503/http://www.mkyong.com/tag/spring/) [wiring](http://web.archive.org/web/20190223073503/http://www.mkyong.com/tag/wiring/)







