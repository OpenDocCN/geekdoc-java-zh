# 弹簧依赖注入(DI)

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-dependency-injection-di/>

在 Spring 框架中，依赖注入(DI)设计模式用于定义对象之间的依赖关系。它有两种主要类型:

*   定型剂注射
*   构造函数注入

## 1.定型剂注射

这是最流行和最简单的 DI 方法，它通过 setter 方法注入依赖。

##### 例子

具有 setter 方法的 helper 类。

```java
 package com.mkyong.output;

import com.mkyong.output.IOutputGenerator;

public class OutputHelper
{
	IOutputGenerator outputGenerator;

	public void setOutputGenerator(IOutputGenerator outputGenerator){
		this.outputGenerator = outputGenerator;
	}

} 
```

一个 bean 配置文件，用于声明 bean 并通过 setter 注入(属性标签)设置依赖关系。

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="OutputHelper" class="com.mkyong.output.OutputHelper">
		<property name="outputGenerator">
			<ref bean="CsvOutputGenerator" />
		</property>
	</bean>

<bean id="CsvOutputGenerator" class="com.mkyong.output.impl.CsvOutputGenerator" />
<bean id="JsonOutputGenerator" class="com.mkyong.output.impl.JsonOutputGenerator" />

</beans> 
```

您只需通过 setter 方法(setOutputGenerator)将“CsvOutputGenerator”bean 注入到“OutputHelper”对象中。

## 2.构造函数注入

这个 DI 方法将通过构造函数注入依赖关系。

##### 例子

带有构造函数的帮助器类。

```java
 package com.mkyong.output;

import com.mkyong.output.IOutputGenerator;

public class OutputHelper
{
	IOutputGenerator outputGenerator;

        OutputHelper(IOutputGenerator outputGenerator){
		this.outputGenerator = outputGenerator;
	}
} 
```

一个 bean 配置文件，用于声明 bean 并通过构造函数注入(constructor-arg 标记)设置依赖关系。

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="OutputHelper" class="com.mkyong.output.OutputHelper">
		<constructor-arg>
			<bean class="com.mkyong.output.impl.CsvOutputGenerator" />
		</constructor-arg>
	</bean>

<bean id="CsvOutputGenerator" class="com.mkyong.output.impl.CsvOutputGenerator" />
<bean id="JsonOutputGenerator" class="com.mkyong.output.impl.JsonOutputGenerator" />

</beans> 
```

您只需通过构造函数将“CsvOutputGenerator”bean 注入到“OutputHelper”对象中。

## Setter 还是 Constructor 注入？

Spring framework 没有硬性规定，只需使用适合您项目需要的任何类型的 DI。然而，由于 setter 注入的简单性，它总是被大多数场景所选择。

## 参考

1.[http://en.wikipedia.org/wiki/Dependency_injection](http://web.archive.org/web/20220316201916/https://en.wikipedia.org/wiki/Dependency_injection)

<input type="hidden" id="mkyong-current-postId" value="3650">