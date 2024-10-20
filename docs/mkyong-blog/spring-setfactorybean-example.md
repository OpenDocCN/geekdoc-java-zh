# Spring SetFactoryBean 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-setfactorybean-example/>

' **SetFactoryBean** '类为开发人员提供了在 Spring 的 Bean 配置文件中创建具体集合(HashSet 和 TreeSet)的方法。

这里有一个 ListFactoryBean 示例，它将在运行时实例化一个 HashSet，并将其注入到一个 Bean 属性中

```java
 package com.mkyong.common;

import java.util.Set;

public class Customer 
{
	private Set sets;
	//...
} 
```

Spring 的 bean 配置文件。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<property name="sets">
			<bean class="org.springframework.beans.factory.config.SetFactoryBean">
				<property name="targetSetClass">
					<value>java.util.HashSet</value>
				</property>
				<property name="sourceSet">
					<list>
						<value>1</value>
						<value>2</value>
						<value>3</value>
					</list>
				</property>
			</bean>
		</property>
	</bean>

</beans> 
```

或者，您也可以使用 util schema 和<set>来实现同样的事情。</set>

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:util="http://www.springframework.org/schema/util"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	http://www.springframework.org/schema/util
	http://www.springframework.org/schema/util/spring-util-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">
		<property name="sets">
			<util:set set-class="java.util.HashSet">
				<value>1</value>
				<value>2</value>
				<value>3</value>
			</util:set>
		</property>
	</bean>

</beans> 
```

记得包括 util 模式，否则您将遇到以下错误

```java
 Caused by: org.xml.sax.SAXParseException: 
	The prefix "util" for element "util:set" is not bound. 
```

运行它…

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = new ClassPathXmlApplicationContext("SpringBeans.xml");

    	Customer cust = (Customer)context.getBean("CustomerBean");
    	System.out.println(cust);

    }
} 
```

输出

```java
 Customer [sets=[3, 2, 1]] Type=[class java.util.HashSet] 
```

您已经实例化了 HashSet 和，并在运行时将其注入到 Customer 的 sets 属性中。

## 下载源代码

Download It – [Spring-SetFactoryBean-Example.zip](http://web.archive.org/web/20190225101247/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-SetFactoryBean-Example.zip) (5KB) ## 参考

1.  [SetFactoryBean Javadoc](http://web.archive.org/web/20190225101247/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/beans/factory/config/SetFactoryBean.html)

[spring](http://web.archive.org/web/20190225101247/http://www.mkyong.com/tag/spring/)![](img/5fc038b61448381f2ba8c041d7061bba.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225101247/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3744">







