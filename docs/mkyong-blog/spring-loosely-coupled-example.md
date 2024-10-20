# 弹簧松耦合示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-loosely-coupled-example/>

面向对象的概念是一个很好的设计，可以把你的系统分成一组可重用的对象。然而，当系统变得越来越大时，尤其是在 Java 项目中，巨大的对象依赖总是紧密耦合的，导致对象很难管理或修改。在这种情况下，您可以使用 Spring framework 作为一个中心模块，轻松有效地管理所有的对象依赖关系。

## 输出生成器示例

我们来看一个例子，假设你的项目有一个功能，把内容输出到 Csv 或者 Json 格式。您的代码可能类似于以下示例:

*文件:IOutputGenerator.java-输出生成器的接口*

```java
 package com.mkyong.output;

public interface IOutputGenerator
{
	public void generateOutput();
} 
```

文件:CsvOutputGenerator.java——实现 IOutputGenerator 接口的 Csv 输出生成器。

```java
 package com.mkyong.output.impl;

import com.mkyong.output.IOutputGenerator;

public class CsvOutputGenerator implements IOutputGenerator
{
	public void generateOutput(){
		System.out.println("Csv Output Generator");
	}
} 
```

文件:JsonOutputGenerator.java——实现 IOutputGenerator 接口的 Json 输出生成器。

```java
 package com.mkyong.output.impl;

import com.mkyong.output.IOutputGenerator;

public class JsonOutputGenerator implements IOutputGenerator
{
	public void generateOutput(){
		System.out.println("Json Output Generator");
	}
} 
```

有几种方法可以调用 IOutputGenerator，以及如何使用 Spring 来避免对象之间的紧密耦合。

 ## 1.方法 1–直接调用它

正常方式，直接调用。

```java
 package com.mkyong.common;

import com.mkyong.output.IOutputGenerator;
import com.mkyong.output.impl.CsvOutputGenerator;

public class App 
{
    public static void main( String[] args )
    {
    	IOutputGenerator output = new CsvOutputGenerator();
    	output.generateOutput();
    }
} 
```

**问题**如果这段代码分散在你的项目中，那么输出生成器的每一个变化都会让你深受其害。

 ## 方法 2–用助手类调用它

您可能会考虑创建一个助手类，将所有输出实现都放入其中。

```java
 package com.mkyong.output;

import com.mkyong.output.IOutputGenerator;
import com.mkyong.output.impl.CsvOutputGenerator;

public class OutputHelper
{
	IOutputGenerator outputGenerator;

	public OutputHelper(){
		outputGenerator = new CsvOutputGenerator();
	}

	public void generateOutput(){
		outputGenerator.generateOutput();
	}

} 
```

通过助手类调用它。

```java
 package com.mkyong.common;

import com.mkyong.output.OutputHelper;

public class App 
{
    public static void main( String[] args )
    {
    	OutputHelper output = new OutputHelper();
    	output.generateOutput(); 
    }
} 
```

**问题**
这看起来更优雅，你只需要管理一个助手类，然而助手类仍然与 CsvOutputGenerator 紧密耦合，输出生成器的每一个变化仍然涉及微小的代码变化。

## 方法 3–弹簧

在这种情况下，Spring 依赖注入(DI)是一个不错的选择。Spring 可以让你的输出生成器松耦合到输出生成器。

OutputHelper 类中的微小变化。

```java
 package com.mkyong.output;

import com.mkyong.output.IOutputGenerator;

public class OutputHelper
{
	IOutputGenerator outputGenerator;

	public void generateOutput(){
		outputGenerator.generateOutput();
	}

	public void setOutputGenerator(IOutputGenerator outputGenerator){
		this.outputGenerator = outputGenerator;
	}
} 
```

创建一个 Spring bean 配置文件，并在这里声明所有的 Java 对象依赖项。

```java
 <!-- Spring-Common.xml -->
<beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="OutputHelper" class="com.mkyong.output.OutputHelper">
		<property name="outputGenerator" ref="CsvOutputGenerator" />
	</bean>

	<bean id="CsvOutputGenerator" class="com.mkyong.output.impl.CsvOutputGenerator" />
	<bean id="JsonOutputGenerator" class="com.mkyong.output.impl.JsonOutputGenerator" />

</beans> 
```

称之为经由春天

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.mkyong.output.OutputHelper;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
    	   new ClassPathXmlApplicationContext(new String[] {"Spring-Common.xml"});

    	OutputHelper output = (OutputHelper)context.getBean("OutputHelper");
    	output.generateOutput();

    }
} 
```

现在，您只需要为不同的输出生成器更改 Spring XML 文件。当输出改变时，您只需要修改 Spring XML 文件，不需要修改代码，这意味着更少的错误。

## 结论

使用 Spring framework——依赖注入(DI)对于对象依赖管理来说是一个有用的特性，它非常优雅、高度灵活并且便于维护，尤其是在大型 Java 项目中。

[spring](http://web.archive.org/web/20190306163900/http://www.mkyong.com/tag/spring/)







