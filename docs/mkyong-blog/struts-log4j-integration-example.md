# Struts + Log4j 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-log4j-integration-example/>

![struts 1 and log4j](img/ee577685ed373e3a20881a4dfb9ec141.png)

在本教程中，我们将向您展示如何将 log4j 框架与传统的 Struts 1.3.x web 应用程序集成。没有额外的工作，只需包含`log4j.jar`并创建一个`log4j.xml`或`log4j.properties`文件，并将其放入类路径的根目录(对于 Maven，将其放入 resources 文件夹)。

使用的技术和工具:

1.  Log4j 1.2.17
2.  Struts 1.3.10
3.  maven3
4.  tomcat6
5.  日食开普勒 4.3

## 1.项目目录

审查最终的项目结构。

![struts1-log4j-directory](img/9d1081eace16143615a1aaad42dd283f.png)

## 2.项目相关性

声明 Struts 和 log4j 依赖关系:

pom.xml

```java
 <properties>
		<struts.version>1.3.10</struts.version>
		<log4j.version>1.2.17</log4j.version>
	</properties>

	<dependencies>

		<!-- Struts 1.x -->
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts-core</artifactId>
			<version>${struts.version}</version>
		</dependency>

		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts-taglib</artifactId>
			<version>${struts.version}</version>
		</dependency>

		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts-extras</artifactId>
			<version>${struts.version}</version>
		</dependency>

		<!-- Log4j -->
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>${log4j.version}</version>
		</dependency>

		<!-- Need this for web -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
		</dependency>
	</dependencies> 
```

## 3.log4j.xml

创建一个 log4j XML 文件，放入`resources`文件夹，参考步骤#1。它告诉 log4j 将日志消息重定向到控制台和一个文件。

log4j.xml

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true"
  xmlns:log4j='http://jakarta.apache.org/log4j/'>

  <!-- Console -->
  <appender name="console" class="org.apache.log4j.ConsoleAppender">
	<layout class="org.apache.log4j.PatternLayout">
	    <param name="ConversionPattern" 
               value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
	</layout>
  </appender>

  <!-- file  -->
  <appender name="file" class="org.apache.log4j.RollingFileAppender">
	<param name="append" value="false" />
	<param name="maxFileSize" value="10KB" />
	<param name="maxBackupIndex" value="5" />
	<param name="file" value="${catalina.home}/logs/myStruts1App.log" />
	<layout class="org.apache.log4j.PatternLayout">
	    <param name="ConversionPattern" 
                value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
	</layout>
  </appender>

  <root>
	<level value="DEBUG" />
	<appender-ref ref="console" />
	<appender-ref ref="file" />
  </root>
</log4j:configuration> 
```

## 4.消息记录

一个返回页面的简单操作，并向您展示了如何使用 log4j 进行日志记录。

WelcomeAction.java

```java
 package com.mkyong.common.action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.log4j.Logger;
import org.apache.struts.action.Action;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;

public class WelcomeAction extends Action{

	//Get a logger
	private static final Logger logger = Logger.getLogger(WelcomeAction.class);

	public ActionForward execute(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
        throws Exception {

		//logs debug
		if(logger.isDebugEnabled()){
			logger.debug("WelcomeAction.execute()");
		}

		//logs exception
		logger.error("This is Error message", new Exception("Testing"));

	    return mapping.findForward("success");

	}

} 
```

## 5.Struts 1 配置

简单的 Struts 1 配置等。

web.xml

```java
 <web-app  
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	  http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">

	<display-name>Log4j + Struts Web Application</display-name>

	<servlet>
		<servlet-name>action</servlet-name>
		<servlet-class>
			org.apache.struts.action.ActionServlet
		</servlet-class>
		<init-param>
			<param-name>config</param-name>
			<param-value>
				/WEB-INF/struts-config.xml
			</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>

	</servlet>

	<servlet-mapping>
		<servlet-name>action</servlet-name>
		<url-pattern>*.do</url-pattern>
	</servlet-mapping>

</web-app> 
```

struts-config.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<action-mappings>

		<action
			path="/welcome"
			type="com.mkyong.common.action.WelcomeAction"
			>	

			<forward name="success" path="/pages/welcome.jsp"/>

		</action>
	</action-mappings>

</struts-config> 
```

pages/welcome.jsp

```java
 <%@taglib uri="http://struts.apache.org/tags-bean" prefix="bean"%>
<html>
<head>
</head>
<body>
<h1>
	Struts 1.x + Log4j framework
</h1>
</body>
</html> 
```

## 6.演示

运行 Struts 1 web 应用程序，并访问欢迎操作。

*网址:http://localhost:8080/log 4 jandstruts 1/welcome . do*

![struts1-log4j-demo](img/54548a4f47f92e6be13cd99f119bb041.png)

6.1 Eclipse 控制台。

![struts1-log4j-console](img/558a0805e545b30947ebf31191ddb98f.png)

6.2 此外，将在 Tomcat 的 logs 文件夹中创建一个日志文件。

![struts1-log4j-file](img/d5da392c71ba0871c143fa550afc75a5.png)

*图:D:\ Apache-Tomcat-6 . 0 . 37 \ logs \ mystruts 1 app . log*

## 下载源代码

Download it – [Log4jAndStrutsExample.zip](http://web.archive.org/web/20220618160428/http://www.mkyong.com/wp-content/uploads/2010/04/Log4jAndStrutsExample.zip) (11 KB)

## 参考

1.  [log4j 1.2 官方页面](http://web.archive.org/web/20220618160428/https://logging.apache.org/log4j/1.2/)
2.  [log4j hello world 示例](http://web.archive.org/web/20220618160428/http://www.mkyong.com/logging/log4j-hello-world-example/)
3.  [Struts 2 异常和日志记录](http://web.archive.org/web/20220618160428/https://www.packtpub.com/article/exceptions-and-logging-in-apache-struts2)

<input type="hidden" id="mkyong-current-postId" value="4629">