# Wicket + Spring 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/wicket-spring-integration-example/>

本教程演示了如何将 Wicket 与 Spring 框架集成在一起。

本文中的库:

1.  Wicket v1.4.17
2.  小门弹簧 v1.4.17
3.  Spring v3.0.5.RELEASE

## 1.项目结构

本教程的最终项目目录结构，没什么特别的，只是一个标准的 Maven 项目。



![wicket spring example](img/fa27945bc4173afa430b696ca1ce7419.png "wicket-spring-example")

## 2.项目依赖性

获取 Wicket 和 Spring 的依赖关系，要集成两者，需要" **wicket-spring.jar** "。

```java
 <project ..>

	<dependencies>

		<!-- Wicket framework-->
		<dependency>
			<groupId>org.apache.wicket</groupId>
			<artifactId>wicket</artifactId>
			<version>1.4.17</version>
		</dependency>

		<!-- Integrate Wicket with Spring -->
		<dependency>
			<groupId>org.apache.wicket</groupId>
			<artifactId>wicket-spring</artifactId>
			<version>1.4.17</version>
		</dependency>

		<!-- Spring framework -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>3.0.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>3.0.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>3.0.5.RELEASE</version>
		</dependency>

		<!-- slf4j-log4j -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.5.6</version>
		</dependency>

	</dependencies>

</project> 
```

## 3.春豆

创建一个 Spring bean，用 **@Service** 对其进行注释。

```java
 package com.mkyong.user;

public interface HelloService {

	String getHelloWorldMsg();

} 
```

```java
 package com.mkyong.user;

import org.springframework.stereotype.Service;

@Service
public class HelloServiceImpl implements HelloService {

	public String getHelloWorldMsg() {
		return "Spring : hello world";
	}

} 
```

## 4.注入弹簧容器

创建一个标准的 Spring**application context . XML**文件，启用自动组件扫描特性。

*文件:applicationContext.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context-3.0.xsd">

	<context:component-scan base-package="com.mkyong.user" />

</beans> 
```

## 5.带弹簧的集成小门

用这个“`addComponentInstantiationListener(new SpringComponentInjector(this));`”覆盖 Wicket 应用程序 **init()** 方法。

*文件:Wicket 应用程序类*

```java
 package com.mkyong;

import org.apache.wicket.protocol.http.WebApplication;
import org.apache.wicket.spring.injection.annot.SpringComponentInjector;
import com.mkyong.user.SimplePage;

public class WicketApplication extends WebApplication {

	@Override
	public Class<SimplePage> getHomePage() {

		return SimplePage.class; // return default page
	}

	@Override
	protected void init() {

		super.init();
		addComponentInstantiationListener(new SpringComponentInjector(this));

	}

} 
```

现在，您可以通过 **@SpringBean** 将 Spring bean 注入 Wicket 组件。

```java
 package com.mkyong.user;

import org.apache.wicket.PageParameters;
import org.apache.wicket.markup.html.basic.Label;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.spring.injection.annot.SpringBean;

public class SimplePage extends WebPage {

	@SpringBean
	private HelloService helloService;

	public SimplePage(final PageParameters parameters) {

		add(new Label("msg", helloService.getHelloWorldMsg()));

	}

} 
```

```java
 <html>
<body>
	<h1>Wicket + Spring integration example</h1>

	<h2 wicket:id="msg"></h2>

</body>
</html> 
```

## 6.web.xml

最后一步，让你的 web 项目知道什么是 Wicket 和 Spring。在`web.xml`中声明两者。

*文件:web.xml*

```java
 <?xml version="1.0" encoding="ISO-8859-1"?>
<web-app  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
	http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"
	version="2.4">
	<display-name>Wicket Web Application</display-name>

	<filter>
		<filter-name>wicket.wicketTest</filter-name>
		<filter-class>org.apache.wicket.protocol.http.WicketFilter</filter-class>
		<init-param>
			<param-name>applicationClassName</param-name>
			<param-value>com.mkyong.WicketApplication</param-value>
		</init-param>
	</filter>

	<filter-mapping>
		<filter-name>wicket.wicketTest</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	<listener>
		<listener-class>
                     org.springframework.web.context.ContextLoaderListener
                </listener-class>
	</listener>

</web-app> 
```

## 7.演示

开始并访问-*http://localhost:8080/wicket examples/*。

一个简单的 Wicket 页面，消息从 Spring 返回。



![wicket spring demo](img/122b885c5081da89a0fd1e750efdd2f3.png "wicket-spring-example-demo")Download it – [Wicket-Spring-Integration-Example.zip](http://web.archive.org/web/20210110024944/http://www.mkyong.com/wp-content/uploads/2011/06/Wicket-Spring-Integration-Example.zip) (8KB)

## 参考

1.  [小门+弹簧详细说明](http://web.archive.org/web/20210110024944/https://cwiki.apache.org/WICKET/spring.html)
2.  [Spring Maven 详情](http://web.archive.org/web/20210110024944/http://blog.springsource.com/2009/12/02/obtaining-spring-3-artifacts-with-maven/)
3.  [弹簧自动组件扫描](http://web.archive.org/web/20210110024944/http://www.mkyong.com/spring/spring-auto-scanning-components/)

Tags : [integration](http://web.archive.org/web/20210110024944/https://mkyong.com/tag/integration/) [spring](http://web.archive.org/web/20210110024944/https://mkyong.com/tag/spring/) [wicket](http://web.archive.org/web/20210110024944/https://mkyong.com/tag/wicket/)<input type="hidden" id="mkyong-current-postId" value="9121">

### 相关文章

*   [Wicket + Log4j 集成示例](/web/20210110024944/https://www.mkyong.com/wicket/wicket-log4j-integration-example/)
*   [春天+ JDBC 的例子](/web/20210110024944/https://www.mkyong.com/spring/maven-spring-jdbc-example/)
*   [Spring+JDBC template+JdbcDaoSupport 示例](/web/20210110024944/https://www.mkyong.com/spring/spring-jdbctemplate-jdbcdaosupport-examples/)
*   [Spring + JDK 定时器调度器示例](/web/20210110024944/https://www.mkyong.com/spring/spring-jdk-timer-scheduler-example/)
*   [Spring 3 + Quartz 1.8.6 调度器示例](/web/20210110024944/https://www.mkyong.com/spring/spring-quartz-scheduler-example/)

*   [Struts + Spring 集成示例](/web/20210110024944/https://www.mkyong.com/struts/struts-spring-integration-example/)
*   [Struts + Spring + Hibernate 集成示例](/web/20210110024944/https://www.mkyong.com/struts/struts-spring-hibernate-integration-example/)
*   [Struts 1+Spring 2 . 5 . 6+Quartz 1.6 调度器 exa](/web/20210110024944/https://www.mkyong.com/struts/struts-spring-quartz-scheduler-integration-example/)
*   [Struts 2 + Spring 集成示例](/web/20210110024944/https://www.mkyong.com/struts2/struts-2-spring-integration-example/)
*   [Struts 2+Spring 2 . 5 . 6+Quartz 1.6 调度器 int](/web/20210110024944/https://www.mkyong.com/struts2/struts-2-spring-quartz-scheduler-integration-example/)