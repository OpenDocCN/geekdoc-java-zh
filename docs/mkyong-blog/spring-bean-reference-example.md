# Spring bean 参考示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-bean-reference-example/>

在 Spring 中，bean 可以通过在相同或不同的 bean 配置文件中指定 bean 引用来“访问”彼此。

## 1.不同 XML 文件中的 Bean

如果您在不同的 XML 文件中引用一个 bean，您可以用一个'`ref`'标记、'`bean`'属性来引用它。

```java
 <ref bean="someBean"/> 
```

在本例中，在'`Spring-Common.xml`'中声明的 bean " **OutputHelper** "可以访问'`Spring-Output.xml`'–"**CsvOutputGenerator**"或" **JsonOutputGenerator** "中的其他 bean，方法是使用 property 标记中的' ref '属性。

*文件:Spring-Common.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="OutputHelper" class="com.mkyong.output.OutputHelper">
		<property name="outputGenerator" >
			<ref bean="CsvOutputGenerator"/>
		</property>
	</bean>

</beans> 
```

*文件:Spring-Output.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="CsvOutputGenerator" class="com.mkyong.output.impl.CsvOutputGenerator" />
	<bean id="JsonOutputGenerator" class="com.mkyong.output.impl.JsonOutputGenerator" />

</beans> 
```

 ## 2.同一 XML 文件中的 Bean

如果在同一个 XML 文件中引用一个 bean，可以用'`ref`'标记、'`local`'属性引用它。

```java
 <ref local="someBean"/> 
```

在本例中，'`Spring-Common.xml`'中声明的 bean " **OutputHelper** "可以互相访问" **CsvOutputGenerator** "或" **JsonOutputGenerator** "。

*文件:Spring-Common.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="OutputHelper" class="com.mkyong.output.OutputHelper">
		<property name="outputGenerator" >
			<ref local="CsvOutputGenerator"/>
		</property>
	</bean>

	<bean id="CsvOutputGenerator" class="com.mkyong.output.impl.CsvOutputGenerator" />
	<bean id="JsonOutputGenerator" class="com.mkyong.output.impl.JsonOutputGenerator" />

</beans> 
```

 ## 结论

实际上,“ref”标记可以访问相同或不同 XML 文件中的 bean，但是，为了项目的可读性，如果引用在相同 XML 文件中声明的 bean，则应该使用“local”属性。

[spring](http://web.archive.org/web/20190311141827/http://www.mkyong.com/tag/spring/)







