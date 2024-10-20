# Spring 集合(列表、集合、映射和属性)示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-collections-list-set-map-and-properties-example/>

Spring 示例向您展示了如何将值注入集合类型(列表、集合、映射和属性)。支持 4 种主要集合类型:

*   列表—
*   设置-
*   地图—<map></map>
*   属性—

## 春豆

一个客户对象，具有四个集合属性。

```java
 package com.mkyong.common;

import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

public class Customer 
{
	private List<Object> lists;
	private Set<Object> sets;
	private Map<Object, Object> maps;
	private Properties pros;

	//...
} 
```

查看不同的代码片段以在 bean 配置文件中声明集合。

 ## 1.列表示例

```java
 <property name="lists">
		<list>
			<value>1</value>
			<ref bean="PersonBean" />
			<bean class="com.mkyong.common.Person">
				<property name="name" value="mkyongList" />
				<property name="address" value="address" />
				<property name="age" value="28" />
			</bean>
		</list>
	</property> 
```

 ## 2.树立榜样

```java
 <property name="sets">
		<set>
			<value>1</value>
			<ref bean="PersonBean" />
			<bean class="com.mkyong.common.Person">
				<property name="name" value="mkyongSet" />
				<property name="address" value="address" />
				<property name="age" value="28" />
			</bean>
		</set>
	</property> 
```

## 3.地图示例

```java
 <property name="maps">
		<map>
			<entry key="Key 1" value="1" />
			<entry key="Key 2" value-ref="PersonBean" />
			<entry key="Key 3">
				<bean class="com.mkyong.common.Person">
					<property name="name" value="mkyongMap" />
					<property name="address" value="address" />
					<property name="age" value="28" />
				</bean>
			</entry>
		</map>
	</property> 
```

## 4.属性示例

```java
 <property name="pros">
		<props>
			<prop key="admin">admin@nospam.com</prop>
			<prop key="support">support@nospam.com</prop>
		</props>
	</property> 
```

完整的 Spring 的 bean 配置文件。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CustomerBean" class="com.mkyong.common.Customer">

		<!-- java.util.List -->
		<property name="lists">
			<list>
				<value>1</value>
				<ref bean="PersonBean" />
				<bean class="com.mkyong.common.Person">
					<property name="name" value="mkyongList" />
					<property name="address" value="address" />
					<property name="age" value="28" />
				</bean>
			</list>
		</property>

		<!-- java.util.Set -->
		<property name="sets">
			<set>
				<value>1</value>
				<ref bean="PersonBean" />
				<bean class="com.mkyong.common.Person">
					<property name="name" value="mkyongSet" />
					<property name="address" value="address" />
					<property name="age" value="28" />
				</bean>
			</set>
		</property>

		<!-- java.util.Map -->
		<property name="maps">
			<map>
				<entry key="Key 1" value="1" />
				<entry key="Key 2" value-ref="PersonBean" />
				<entry key="Key 3">
					<bean class="com.mkyong.common.Person">
						<property name="name" value="mkyongMap" />
						<property name="address" value="address" />
						<property name="age" value="28" />
					</bean>
				</entry>
			</map>
		</property>

		<!-- java.util.Properties -->
		<property name="pros">
			<props>
				<prop key="admin">admin@nospam.com</prop>
				<prop key="support">support@nospam.com</prop>
			</props>
		</property>

	</bean>

	<bean id="PersonBean" class="com.mkyong.common.Person">
		<property name="name" value="mkyong1" />
		<property name="address" value="address 1" />
		<property name="age" value="28" />
	</bean>

</beans> 
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
 Customer [

lists=[
1, 
Person [address=address 1, age=28, name=mkyong1], 
Person [address=address, age=28, name=mkyongList]
], 

maps={
key 1=1,
key 2=Person [address=address 1, age=28, name=mkyong1], 
key 3=Person [address=address, age=28, name=mkyongMap]
}, 

pros={admin=admin@nospam.com, support=support@nospam.com}, 

sets=[
1, 
Person [address=address 1, age=28, name=mkyong1], 
Person [address=address, age=28, name=mkyongSet]]
] 
```

## 下载源代码

Download it – [Spring-Collection-Example.zip](http://web.archive.org/web/20190223081346/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Collection-Example.zip) (6 KB)[spring](http://web.archive.org/web/20190223081346/http://www.mkyong.com/tag/spring/)







