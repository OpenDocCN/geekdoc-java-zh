# JSF 2 + Log4j 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-log4j-integration-example/>

![jsf log4j logo](img/7a7be50de79b7d4831a643107cc83123.png)

在本教程中，我们将向您展示如何将 log4j 框架与 JSF 2.x web 应用程序集成。JSF 正在使用`java.util.logging`，您需要额外的工作来将日志从 JSF 的`java.util.logging`重定向到`log4j`，这会带来严重的[性能损失](http://web.archive.org/web/20220618160448/http://www.slf4j.org/legacy.html#jul-to-slf4j)，请确保仅在本地开发或调试环境中使用此技巧。

检查项目环境:

1.  SLF4j 1.7.7
2.  Log4j 1.2.17
3.  JSF 2.2.7
4.  maven3
5.  tomcat6
6.  日食开普勒 4.3

简而言之，集成 JSF 和 log4j 的步骤。

1.  在`logging.properties`上打开 JSF 日志，因为这个项目是在 Eclipse 环境中运行的，所以将使用 Eclipse 的`JRE/lib/logging.properties`。
2.  创建一个`log4j.properties`并把它放在类路径上。
3.  创建一个 Servlet 的监听器，安装`slf4j bridge handler`将日志从 java.util.logging 重定向到 log4j。

**Note**
Refer to this [SLF4j Bridging legacy APIs](http://web.archive.org/web/20220618160448/http://www.slf4j.org/legacy.html)

## 1.项目目录

审查最终的项目结构。

![jsf-log4j-directory](img/95b50ed336743f23c146c5707ca910df.png)

## 2.项目相关性

你需要 slf4j-log4j12 和 jul-to-slf4j。

pom.xml

```java
 <properties>
		<jdk.version>1.7</jdk.version>
		<jsf.version>2.2.7</jsf.version>
		<slf4j.version>1.7.7</slf4j.version>
	</properties>

	<dependencies>

		<!-- from java.util.logging to log4j -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jul-to-slf4j</artifactId>
			<version>${slf4j.version}</version>
		</dependency>

		<!-- it help to get slf4j and log4j -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${slf4j.version}</version>
		</dependency>

		<!-- JSF -->
		<dependency>
			<groupId>com.sun.faces</groupId>
			<artifactId>jsf-api</artifactId>
			<version>${jsf.version}</version>
		</dependency>
		<dependency>
			<groupId>com.sun.faces</groupId>
			<artifactId>jsf-impl</artifactId>
			<version>${jsf.version}</version>
		</dependency>

		<!-- you need this in web -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
		</dependency>

	</dependencies> 
```

## 4.JSF 伐木公司

因为这个项目是在 Eclipse 的环境中运行的，所以将使用 Eclipse 的 JRE。打开`logging.properties`，将等级改为最细，打开`javax.faces`和`com.sun.faces`上的测井:

${JRE_PATH}/lib/logging.properties

```java
 # Default is INFO, change it to FINNEST
#java.util.logging.ConsoleHandler.level = INFO

java.util.logging.ConsoleHandler.level = FINEST
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter

#Add these two lines to enable JSF logging
javax.faces.level = FINEST
com.sun.faces.level = FINEST 
```

参考 [java.util.logging levels](http://web.archive.org/web/20220618160448/https://docs.oracle.com/javase/7/docs/api/java/util/logging/Level.html) 。

**Note**
If this project is deployed on Tomcat directly, update this `${Tomcat}\conf\logging.properties`

## 5.Log4j 记录

创建一个 log4j 属性文件，放入`resources`文件夹，参考步骤#1。

log4j.properties

```java
 # Root logger option
log4j.rootLogger=WARN, console, file

#enable JSF logging
log4j.logger.javax.faces=DEBUG
log4j.logger.com.sun.faces=DEBUG

# Redirect log messages to console
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.Target=System.out
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

# Redirect log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=${catalina.home}/logs/jsfapp.log
log4j.appender.file.MaxFileSize=5KB
log4j.appender.file.MaxBackupIndex=5
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n 
```

## 6.ServletContextListener

创建一个 Servlet 监听器来安装桥处理器，以便将 JSF 的日志从`java.util.properties`重定向到`log4j`。

MyListener

```java
 package com.mkyong.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import org.slf4j.bridge.SLF4JBridgeHandler;

public class MyListener implements ServletContextListener {

	@Override
	public void contextInitialized(ServletContextEvent arg) {
		System.out.println("contextInitialized....");

		//remove the jsf root logger, avoid duplicated logging
		//try comment out this and see the different on the console
		SLF4JBridgeHandler.removeHandlersForRootLogger();
		SLF4JBridgeHandler.install();
	}

	@Override
	public void contextDestroyed(ServletContextEvent arg) {
		System.out.println("contextDestroyed....");

	}

} 
```

包括侦听器类以及其他标准 JSF 设置。

web.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

	xmlns:web="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	id="WebApp_ID" version="2.5">
	<display-name>JavaServerFacesAndLog4j</display-name>

	<context-param>
		<param-name>javax.faces.PROJECT_STAGE</param-name>
		<param-value>Development</param-value>
	</context-param>
	<welcome-file-list>
		<welcome-file>faces/index.xhtml</welcome-file>
	</welcome-file-list>

	<!-- Install slf4j bridge handler -->
	<listener>
		<listener-class>
		    	com.mkyong.listener.MyListener
		  </listener-class>
	</listener>

	<servlet>
		<servlet-name>Faces Servlet</servlet-name>
		<servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>/faces/*</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>*.jsf</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>*.faces</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>*.xhtml</url-pattern>
	</servlet-mapping>
</web-app> 
```

## 7.应用程序日志

一个简单的例子向你展示如何做 log4j 的日志。

WelcomeAction.java

```java
 package com.mkyong.controller;

import java.io.Serializable;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import org.apache.log4j.Logger;

@ManagedBean
@SessionScoped
public class PageController implements Serializable {

	private static final long serialVersionUID = 1L;

	private static final Logger logger = Logger.getLogger(PageController.class);

	public String process() {

		// logs debug
		if (logger.isDebugEnabled()) {
			logger.debug("PageController.process()");
		}

		// logs exception
		logger.error("This is Error message", new Exception("Testing"));

		return "success";
	}

} 
```

一个简单的 JSF 视图资源。

pages/welcome.jsp

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html 
	xmlns:f="http://java.sun.com/jsf/core"
	xmlns:h="http://java.sun.com/jsf/html">

<h:body>
	<h1>JSF 2.0 + Log4j</h1>
	<h:form>
		<h:commandButton action="#{pageController.process}" value="Click Me" />
	</h:form>
</h:body>
</html> 
```

## 8.演示

运行 JSF web 应用程序，并访问默认页面，然后单击按钮。

网址:*http://localhost:8080/Java server facesandlog 4j/*

![jsf-log4j-demo](img/dcee31d2844beeaa1e01d9ba4488eb17.png)

*8.1*JSF 和应用程序记录都将显示在控制台中。

![jsf logging in console](img/025ef3c4e8c7408d9d26633e878e4d07.png)

*图:Eclipse 控制台*

*8.2*JSF 和应用程序日志都将记录在`${tomcat}\logs\jsfapp.log`中。

![jsf logging in file](img/378b320a3db9c483c4e07133ee2dd20c.png)

*图:D:\ Apache-Tomcat-6 . 0 . 37 \ logs \ JSF app . log*

## 下载源代码

Download it – [JSFAndLog4j.zip](http://web.archive.org/web/20220618160448/http://www.mkyong.com/wp-content/uploads/2014/07/JSFAndLog4j.zip) (12 KB)

## 参考

1.  Stackover:为什么水平。未显示详细日志记录消息？
2.  [Stackover : JSF2 日志与 tomcat](http://web.archive.org/web/20220618160448/https://stackoverflow.com/questions/7363704/jsf2-logs-with-tomcat)
3.  [将 java.util.logging 桥接到 SLF4J](http://web.archive.org/web/20220618160448/http://blog.cn-consult.dk/2009/03/bridging-javautillogging-to-slf4j.html)
4.  [log4j 1.2 官方页面](http://web.archive.org/web/20220618160448/https://logging.apache.org/log4j/1.2/)
5.  [log4j hello world 示例](http://web.archive.org/web/20220618160448/http://www.mkyong.com/logging/log4j-hello-world-example/)
6.  [Slf4j:桥接遗留 API](http://web.archive.org/web/20220618160448/http://www.slf4j.org/legacy.html)
7.  [JavaDoc:Java . util . logging](http://web.archive.org/web/20220618160448/https://docs.oracle.com/javase/6/docs/api/java/util/logging/package-summary.html)

<input type="hidden" id="mkyong-current-postId" value="13378">