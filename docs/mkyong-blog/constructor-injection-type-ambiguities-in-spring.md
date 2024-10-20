# Spring 中的构造函数注入类型不明确

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/constructor-injection-type-ambiguities-in-spring/>

在 Spring 框架中，当你的类包含多个参数个数相同的构造函数时，总是会导致**构造函数注入参数类型歧义**的问题。

## 问题

让我们看看这个客户 bean 的例子。它包含两个构造函数方法，都接受 3 个不同数据类型的参数。

```java
 package com.mkyong.common;

public class Customer 
{
	private String name;
	private String address;
	private int age;

	public Customer(String name, String address, int age) {
		this.name = name;
		this.address = address;
		this.age = age;
	}

	public Customer(String name, int age, String address) {
		this.name = name;
		this.age = age;
		this.address = address;
	}
	//getter and setter methods
	public String toString(){
		return " name : " +name + "\n address : "
               + address + "\n age : " + age;
	}

} 
```

在 Spring bean 配置文件中，为名称传递“mkyong”，为地址传递“188”，为年龄传递“28”。

```java
 <!--Spring-Customer.xml-->
<beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">

		<constructor-arg>
			<value>mkyong</value>
		</constructor-arg>

		<constructor-arg>
			<value>188</value>
		</constructor-arg>

		<constructor-arg>
			<value>28</value>
		</constructor-arg>
        </bean>

</beans> 
```

运行它，你的预期结果是什么？

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

输出

```java
 name : mkyong
 address : 28
 age : 188 
```

结果并不是我们所期望的，运行的是第二个构造函数，而不是第一个构造函数。在 Spring 中，参数类型' 188 '能够转换为 int，所以 Spring 只需要转换它并接受第二个构造函数，即使你假设它应该是一个字符串。

此外，如果 Spring 不能解析使用哪个构造函数，它将提示以下错误消息

```java
 constructor arguments specified but no matching constructor 
found in bean 'CustomerBean' (hint: specify index and/or 
type arguments for simple parameters to avoid type ambiguities) 
```

 ## 解决办法

要解决这个问题，您应该始终通过 type 属性为构造函数指定确切的数据类型，如下所示:

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">

		<constructor-arg type="java.lang.String">
			<value>mkyong</value>
		</constructor-arg>

		<constructor-arg type="java.lang.String">
			<value>188</value>
		</constructor-arg>

		<constructor-arg type="int">
			<value>28</value>
		</constructor-arg>

	</bean>

</beans> 
```

再次运行，现在你得到了你所期望的。

##### 输出

```java
 name : mkyong
 address : 188
 age : 28 
```

**Note**
It’s always a good practice to explicitly declared the data type for each constructor argument, to avoid constructor injection type ambiguities issue above.[spring](http://web.archive.org/web/20190309054054/http://www.mkyong.com/tag/spring/)![](img/306930b314782ee346296b3768090b1a.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190309054054/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3654">







