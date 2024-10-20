# TestNG + Spring 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/testng-spring-integration-example/>

在本教程中，我们将向您展示如何用 TestNG 测试 Spring 的组件。

使用的工具:

1.  测试 6.8.7
2.  弹簧 3.2.2 释放
3.  maven3
4.  Eclipse IDE

## 1.项目相关性

要将 Spring 与 TestNG 集成，您需要`spring-test.jar`，添加以下内容:

pom.xml

```java
 <properties>
		<spring.version>3.2.2.RELEASE</spring.version>	
		<testng.version>6.8.7</testng.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.testng</groupId>
			<artifactId>testng</artifactId>
			<version>${testng.version}</version>
			<scope>test</scope>
		</dependency>
	</dependencies> 
```

 ## 2.弹簧组件

创建一个简单的 Spring 组件，稍后我们将使用 TestNG 测试这个组件。

EmailGenerator.java

```java
 package com.mkyong.testng.project.service.email;

public interface EmailGenerator {

	String generate();

} 
```

RandomEmailGenerator.java

```java
 package com.mkyong.testng.project.service.email;

import org.springframework.stereotype.Service;

@Service
public class RandomEmailGenerator implements EmailGenerator {

	@Override
	public String generate() {
		return "feedback@yoursite.com";
	}

} 
```

 ## 3.测试+弹簧

在测试文件夹中创建一个 Spring 配置文件，用于 Spring 组件扫描。

${project}/src/test/resources/spring-test-config.xml

```java
 <beans 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
    http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.2.xsd
	">

	<context:component-scan base-package="com.mkyong.testng" />

</beans> 
```

要访问 TestNG 中的弹簧组件，请扩展`AbstractTestNGSpringContextTests`，参见以下示例:

${project}/src/test/java/com/mkyong/testng/examples/spring/TestSpring.java

```java
 package com.mkyong.testng.examples.spring;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.testng.AbstractTestNGSpringContextTests;
import org.testng.Assert;
import org.testng.annotations.Test;
import com.mkyong.testng.project.service.email.EmailGenerator;

@Test
@ContextConfiguration(locations = { "classpath:spring-test-config.xml" })
public class TestSpring extends AbstractTestNGSpringContextTests {

	@Autowired
	EmailGenerator emailGenerator;

	@Test()
	void testEmailGenerator() {

		String email = emailGenerator.generate();
		System.out.println(email);

		Assert.assertNotNull(email);
		Assert.assertEquals(email, "feedback@yoursite.com");

	}

} 
```

输出

```java
 feedback@yoursite.com
PASSED: testEmailGenerator

===============================================
    Default test
    Tests run: 1, Failures: 0, Skips: 0
=============================================== 
```

## 下载源代码

Download it – [TestNG-Spring-Example.zip](http://web.archive.org/web/20190310100527/http://www.mkyong.com/wp-content/uploads/2014/01/TestNG-Spring-Example.zip) (35 KB)

## 参考

1.  [Spring–TestNG 支持类](http://web.archive.org/web/20190310100527/http://docs.spring.io/spring/docs/3.2.6.RELEASE/spring-framework-reference/htmlsingle/#testcontext-support-classes-testng)
2.  [Spring AbstractTestNGSpringContextTests JavaDoc](http://web.archive.org/web/20190310100527/http://docs.spring.io/spring/docs/3.2.6.RELEASE/javadoc-api/org/springframework/test/context/testng/AbstractTestNGSpringContextTests.html)

[spring](http://web.archive.org/web/20190310100527/http://www.mkyong.com/tag/spring/) [testng](http://web.archive.org/web/20190310100527/http://www.mkyong.com/tag/testng/)







