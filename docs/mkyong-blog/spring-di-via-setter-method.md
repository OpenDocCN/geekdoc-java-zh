# 通过 setter 方法的 Spring DI

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-di-via-setter-method/>

一个简单的 Spring 示例，向您展示了如何通过 setter 方法(最常用的 DI 方法)依赖注入 bean。

## 1.IOutputGenerator

一个接口和它的实现类。

```java
 package com.mkyong.output;

public interface IOutputGenerator
{
	public void generateOutput();
} 
```

```java
 package com.mkyong.output.impl;

import com.mkyong.output.IOutputGenerator;

public class CsvOutputGenerator implements IOutputGenerator {
	public void generateOutput() {
		System.out.println("This is Csv Output Generator");
	}
} 
```

 ## 2.助手类

一个助手类，稍后使用 Spring 来定义 IOutputGenerator。

```java
 package com.mkyong.output;

import com.mkyong.output.IOutputGenerator;

public class OutputHelper {
	IOutputGenerator outputGenerator;

	public void generateOutput() {
		outputGenerator.generateOutput();
	}

	//DI via setter method
	public void setOutputGenerator(IOutputGenerator outputGenerator) {
		this.outputGenerator = outputGenerator;
	}
} 
```

 ## 3.弹簧配置

在 Spring 配置文件中配置 bean，通过 property 标签，ref 属性，将 bean“CsvOutputGenerator”引用到“OutputHelper”中。

在这种情况下，Spring 会将 bean“CsvOutputGenerator”DI 为“OutputHelper”类，通过 setter 方法“**setOutputGenerator(IOutputGenerator output generator**)”。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="OutputHelper" class="com.mkyong.output.OutputHelper">
		<property name="outputGenerator" ref="CsvOutputGenerator" />
	</bean>

	<bean id="CsvOutputGenerator" class="com.mkyong.output.impl.CsvOutputGenerator" />

</beans> 
```

## 4.运行它

加载所有内容，并运行它。

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.mkyong.output.OutputHelper;

public class App {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext(
				"SpringBeans.xml");

		OutputHelper output = (OutputHelper)context.getBean("OutputHelper");
    	        output.generateOutput();
	}
} 
```

输出

```java
 This is Csv Output Generator 
```

## 下载源代码

Download it – [Spring-DI-setter-method-example.zip](http://web.archive.org/web/20190225100442/http://www.mkyong.com/wp-content/uploads/2011/06/Spring-DI-setter-method-example.zip) (6KB)[dependency injection](http://web.archive.org/web/20190225100442/http://www.mkyong.com/tag/dependency-injection/) [spring](http://web.archive.org/web/20190225100442/http://www.mkyong.com/tag/spring/)







