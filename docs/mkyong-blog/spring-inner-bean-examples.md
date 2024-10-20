# Spring 内部 bean 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-inner-bean-examples/>

在 Spring framework 中，每当 bean 仅用于一个特定属性时，建议将其声明为内部 bean。并且 setter 注入'`property`'和 constructor 注入'`constructor-arg`'都支持内部 bean。

请看一个详细的例子来演示 Spring inner bean 的使用。

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

	@Override
	public String toString() {
		return "Customer [person=" + person + "]";
	}
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

	@Override
	public String toString() {
		return "Person [address=" + address + ", 
                               age=" + age + ", name=" + name + "]";
	}	
} 
```

通常，您可以使用'`ref`'属性将“Person”bean 引用到“Customer”bean 中，Person 属性如下:

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<property name="person" ref="PersonBean" />
	</bean>

	<bean id="PersonBean" class="com.mkyong.common.Person">
		<property name="name" value="mkyong" />
		<property name="address" value="address1" />
		<property name="age" value="28" />
	</bean>

</beans> 
```

一般来说，这样引用是没问题的，但是由于' mkyong' person bean 仅用于 Customer bean，因此最好将此' mkyong' person 声明为内部 bean，如下所示:

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<property name="person">
			<bean class="com.mkyong.common.Person">
				<property name="name" value="mkyong" />
				<property name="address" value="address1" />
				<property name="age" value="28" />
			</bean>
		</property>
	</bean>
</beans> 
```

该内部 bean 在构造函数注入中也受支持，如下所示:

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<constructor-arg>
			<bean class="com.mkyong.common.Person">
				<property name="name" value="mkyong" />
				<property name="address" value="address1" />
				<property name="age" value="28" />
			</bean>
		</constructor-arg>
	</bean>
</beans> 
```

**Note**
The id or name value in bean class is not necessary in an inner bean, it will simply ignored by the Spring container.

运行它

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
    	  new ClassPathXmlApplicationContext(new String[] {"Spring-Customer.xml"});

    	Customer cust = (Customer)context.getBean("CustomerBean");
    	System.out.println(cust);

    }
} 
```

*输出*

```java
 Customer [person=Person [address=address1, age=28, name=mkyong]] 
```

[spring](http://web.archive.org/web/20190217092743/http://www.mkyong.com/tag/spring/)![](img/db06ed711781699d407f3a50e47f659a.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190217092743/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3663">







