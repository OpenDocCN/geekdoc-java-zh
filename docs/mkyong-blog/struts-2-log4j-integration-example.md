# Struts 2 + Log4j 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-log4j-integration-example/>

![struts2 log4j](img/fe593cb86859a914663652c786ceac96.png)

在本教程中，我们将向您展示如何将 log4j 框架与 Struts 2 web 应用程序集成。你需要做的就是

1.  包含`log4j.jar`作为项目依赖项
2.  创建一个 log4j.properties 文件，放入类路径的根目录，用 Maven 放入`resources`文件夹。

使用的技术和工具:

1.  Log4j 1.2.17
2.  struts 2.3.16.3
3.  maven3
4.  tomcat6
5.  日食开普勒 4.3

## 1.项目目录

审查最终的项目结构。

![strruts2-log4j-directory](img/b5916e04098a8b78d030a712e12c9018.png)

## 2.项目相关性

声明 Struts 2 和 log4j 依赖关系:

pom.xml

```java
 <project  
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
	http://maven.apache.org/maven-v4_0_0.xsd">

	<modelVersion>4.0.0</modelVersion>
	<groupId>com.mkyong.common</groupId>
	<artifactId>Struts2</artifactId>
	<packaging>war</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>Struts + Log4j Webapp</name>
	<url>http://maven.apache.org</url>

	<properties>
		<jdk.version>1.7</jdk.version>
		<struts.version>2.3.16.3</struts.version>
		<log4j.version>1.2.17</log4j.version>
	</properties>

	<dependencies>

		<!-- Struts 2 -->
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-core</artifactId>
			<version>${struts.version}</version>
		</dependency>

		<!-- Log4j -->
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>${log4j.version}</version>
		</dependency>

	</dependencies>

	<build>
	  <finalName>Struts2</finalName>
	  <plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-eclipse-plugin</artifactId>
			<version>2.9</version>
			<configuration>
				<downloadSources>true</downloadSources>
				<downloadJavadocs>false</downloadJavadocs>
				<wtpversion>2.0</wtpversion>
			</configuration>
		</plugin>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>2.3.2</version>
			<configuration>
				<source>${jdk.version}</source>
				<target>${jdk.version}</target>
			</configuration>
		</plugin>
	  </plugins>
	</build>
</project> 
```

## 3.log4j.properties

创建一个 log4j 属性文件，放入`resources`文件夹，参考步骤#1。

log4j.properties

```java
 # Root logger option
log4j.rootLogger=ERROR, stdout, file

# Redirect log messages to console
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

# Redirect log messages to a log file, support rolling backup file.
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=${catalina.home}/logs/mystruts2app.log
log4j.appender.file.MaxFileSize=5MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n 
```

## 4.Struts 2 动作和日志记录

一个返回页面的简单操作，并向您展示了如何使用 log4j 进行消息日志记录。

WelcomeAction.java

```java
 package com.mkyong.common.action;

import org.apache.log4j.Logger;
import com.opensymphony.xwork2.ActionSupport;

public class WelcomeAction extends ActionSupport {

	private static final long serialVersionUID = 1L;

	//get log4j
	private static final Logger logger = Logger.getLogger(WelcomeAction.class);

	public String execute() throws Exception {

		// logs debug message
		if (logger.isDebugEnabled()) {
			logger.debug("execute()!");
		}

		// logs exception
		logger.error("This is Error message", new Exception("Testing"));

		return SUCCESS;

	}
} 
```

## 5.Struts 2 配置

Struts 2 配置和 JSP 页面，如果您感兴趣的话。

struts.xml

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
	<constant name="struts.devMode" value="true" />

	<package name="welcome" namespace="/" extends="struts-default">

		<action name="welcome" class="com.mkyong.common.action.WelcomeAction">
			<result name="success">pages/success.jsp</result>
		</action>

	</package>

</struts> 
```

web.xml

```java
 <web-app  
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">

	<display-name>Struts 2 Web Application</display-name>

	<filter>
		<filter-name>struts2</filter-name>
		<filter-class>
			org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
		</filter-class>
	</filter>

	<filter-mapping>
		<filter-name>struts2</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

</web-app> 
```

pages/success.jsp

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
</head>

<body>
<h1>Struts 2 + Log4j integration example</h1>

</body>
</html> 
```

## 6.演示

运行 Struts 2 web 应用程序，并访问欢迎操作。

*网址:http://localhost:8888/log 4 jandstruts 2/welcome*

![struts 2 log4j demo](img/3936dd3ee273c4bd2cab152935fb21db.png)

*6.1* 所有的记录信息都会显示在控制台上。

![strruts2-log4j-demo-eclipse-console](img/35ee5fa44eabe0fa68e1b59ada8816e9.png)

*图:Eclipse 控制台*

此外，将在 Tomcat 的 logs 文件夹中创建一个日志文件。

![strruts2-log4j-demo-file](img/779660dd9aa5cc3f8fab4d4db5a2071d.png)

*图:D:\ Apache-Tomcat-6 . 0 . 37 \ logs \ mystruts 2 app . log*

## 下载源代码

Download it – [Log4jAndStruts2Example.zip](http://web.archive.org/web/20220618160332/http://www.mkyong.com/wp-content/uploads/2010/07/Log4jAndStruts2Example.zip) (20 KB)

## 参考

1.  [创建 Struts 2 Web 应用示例](http://web.archive.org/web/20220618160332/https://struts.apache.org/release/2.3.x/docs/create-struts-2-web-application-using-maven-to-manage-artifacts-and-to-build-the-application.html)
2.  [log4j 1.2 官方页面](http://web.archive.org/web/20220618160332/https://logging.apache.org/log4j/1.2/)
3.  [log4j hello world 示例](http://web.archive.org/web/20220618160332/http://www.mkyong.com/logging/log4j-hello-world-example/)
4.  [Struts 2 异常和日志记录](http://web.archive.org/web/20220618160332/https://www.packtpub.com/article/exceptions-and-logging-in-apache-struts2)

<input type="hidden" id="mkyong-current-postId" value="6321">