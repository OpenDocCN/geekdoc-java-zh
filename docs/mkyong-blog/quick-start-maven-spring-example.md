# Maven + Spring hello world 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/quick-start-maven-spring-example/>

这个快速指南示例使用 Maven 生成一个简单的 Java 项目结构，并演示了如何检索 Spring bean 和打印“hello world”字符串。

本文中使用的技术:

1.  弹簧 2.5.6
2.  Maven 3.0.3
3.  Eclipse 3.6
4.  1.6.0.13 JDK

**Spring 3 example**
For Spring 3, refer to this [Maven + Spring 3 hello world example](http://web.archive.org/web/20210421083905/http://www.mkyong.com/spring3/spring-3-hello-world-example/).

## 1.用 Maven 生成项目结构

在命令提示符下，发出以下 Maven 命令:

```java
 mvn archetype:generate -DgroupId=com.mkyong.common -DartifactId=SpringExamples 
	-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
```

Maven 将为您生成所有 Java 的标准文件夹结构(除了 resources 文件夹，您需要手动创建它)

## 2.转换为 Eclipse 项目

键入" **mvn eclipse:eclipse** "将新生成的 Maven 样式项目转换为 eclipse 的样式项目。

```java
 mvn eclipse:eclipse 
```

稍后，将转换后的项目导入 Eclipse IDE。

**Create a resources folder**
Create a resources “**/src/main/resources**” folder, the Spring’s bean xml configuration file will put here later. Maven will treat all files under this “resources” folder as resources files, and copy it to output classes automatically.

## 3.添加弹簧依赖项

在 Maven 的 **pom.xml** 文件中添加 Spring 依赖。

*文件:pom.xml*

```java
 <project  
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
	http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.mkyong.common</groupId>
	<artifactId>SpringExamples</artifactId>
	<packaging>jar</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>SpringExamples</name>
	<url>http://maven.apache.org</url>
	<dependencies>

		<!-- Spring framework -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring</artifactId>
			<version>2.5.6</version>
		</dependency>

	</dependencies>
</project> 
```

问题" **mvn eclipse:eclipse** "同样，Maven 将自动下载 Spring 依赖库，并将其放入 Maven 的本地存储库中。同时，Maven 会将下载的库添加到 Eclipse 的**中。用于依赖目的的类路径**。

## 4.Spring bean (Java 类)

在“src/main/Java/com/mkyong/common/hello world . Java”下创建一个普通的 Java 类(HelloWorld.java)。Spring 的 bean 只是一个普通的 Java 类，稍后在 Spring bean 配置文件中声明。

```java
 package com.mkyong.common;

/**
 * Spring bean
 * 
 */
public class HelloWorld {
	private String name;

	public void setName(String name) {
		this.name = name;
	}

	public void printHello() {
		System.out.println("Hello ! " + name);
	}
} 
```

## 5.Spring bean 配置文件

在"**src/main/resources/Spring-module . xml**"处创建一个 XML 文件(Spring-Module.xml)。这是 Spring 的 bean 配置文件，它声明了所有可用的 Spring beans。

*文件:Spring-Module.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="helloBean" class="com.mkyong.common.HelloWorld">
		<property name="name" value="Mkyong" />
	</bean>

</beans> 
```

## 6.检查项目结构

检查并确保文件夹结构如下

![spring hello world example](img/cb01a87973497863b901584885fa019f.png "spring-hello-world-example")

## 7.运行它

运行`App.java`，加载 Spring bean 配置文件( **Spring-Module.xml** ，通过`getBean()`方法检索 Spring bean。

*文件:App.java*

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext(
				"Spring-Module.xml");

		HelloWorld obj = (HelloWorld) context.getBean("helloBean");
		obj.printHello();
	}
} 
```

## 8.输出

```java
 Hello ! Mkyong 
```

## 下载源代码

Download it – [Spring-hello-world-example.zip](http://web.archive.org/web/20210421083905/http://www.mkyong.com/wp-content/uploads/2010/03/SpringExample.zip) (7KB)Tags : [hello world](http://web.archive.org/web/20210421083905/https://mkyong.com/tag/hello-world/) [maven](http://web.archive.org/web/20210421083905/https://mkyong.com/tag/maven/) [spring](http://web.archive.org/web/20210421083905/https://mkyong.com/tag/spring/)<input type="hidden" id="mkyong-current-postId" value="3601">