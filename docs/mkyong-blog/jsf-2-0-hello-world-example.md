# JSF 2.0 hello world 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-0-hello-world-example/>

在本教程中，我们将向您展示如何开发一个 Java server Faces(JSF)2.0 hello world 示例，展示了 JSF 2.0 依赖项列表、基本注释和配置。

## 项目环境

这个 JSF 2.0 示例是使用以下工具和技术构建的

1.  JSF 2.1.7
2.  maven3
3.  Eclipse 3.6
4.  JDK 1.6
5.  Tomcat 6.0.26

首先，查看最终的项目结构，以防您对以后应该在哪里创建相应的文件或文件夹感到困惑。



![jsf2-hello-world-example](img/a43d00cc827d877352279323eb0317ba.png "jsf2-hello-world-example")

## 1.JSF 2.0 依赖项

~~Maven [中央库](http://web.archive.org/web/20201113070916/http://repo1.maven.org/maven2/)只有 JSF 1.2 版本，要获得 **JSF 2.0** ，可能需要从[Java.net 库](http://web.archive.org/web/20201113070916/http://download.java.net/maven/2)下载。~~
maven 中央库是 JSF 库更新到 2.1.7。不再需要以前的 Java.net 存储库。

**对于大多数 Java EE 应用服务器中的 Glassfish**
这样的 Java EE 应用服务器来说，它内置了对 JSF 2.0 的**支持，所以你需要下载单个 JSF API 进行开发。**

```java
 ...
<dependencies>
  <dependency>
    <groupId>javax.faces</groupId>
    <artifactId>jsf-api</artifactId>
    <version>2.0</version>
    <scope>provided</scope>
  </dependency>
</dependencies>
<repositories>
  <repository>
    <id>java.net.m2</id>
    <name>java.net m2 repo</name>
    <url>http://download.java.net/maven/2</url>
  </repository>
</repositories>
... 
```

对于像 Tomcat
这样简单的 servlet 容器来说，这有点麻烦，你需要下载下面的依赖项。

*文件:pom.xml*

```java
 <project  
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
        http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.mkyong.common</groupId>
	<artifactId>JavaServerFaces</artifactId>
	<packaging>war</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>JavaServerFaces Maven Webapp</name>
	<url>http://maven.apache.org</url>

	<dependencies>

		<dependency>
			<groupId>com.sun.faces</groupId>
			<artifactId>jsf-api</artifactId>
			<version>2.1.7</version>
		</dependency>
		<dependency>
			<groupId>com.sun.faces</groupId>
			<artifactId>jsf-impl</artifactId>
			<version>2.1.7</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
		</dependency>
                <!-- Tomcat 6 need this -->
		<dependency>
			<groupId>com.sun.el</groupId>
			<artifactId>el-ri</artifactId>
			<version>1.0</version>
		</dependency>

	</dependencies>

	<build>
		<finalName>JavaServerFaces</finalName>

		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.3.1</version>
				<configuration>
					<source>1.6</source>
					<target>1.6</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project> 
```

**Note**
For more detail about the JSF 2.0 dependencies, please refer to this [official JSF 2.0 release note](http://web.archive.org/web/20201113070916/https://javaserverfaces.java.net/nonav/rlnotes/2.0.0/releasenotes.html).**Warning**
The **el-ri.jar** is an arguable dependency in the Tomcat servlet container, even it’s not stated in the release note, but you need this library to solve the “[JSP version of the container is older than 2.1…](http://web.archive.org/web/20201113070916/http://www.mkyong.com/jsf2/jsf-2-0-tomcat-it-appears-the-jsp-version-of-the-container-is-older-than-2-1/)” error message.**Updated – 21-10-2010**
This “el-ri.jar” is too old, it’s recommended to use the latest “el-impl-2.2.jar”, from [Java.net](http://web.archive.org/web/20201113070916/http://download.java.net/maven/2/org/glassfish/web/el-impl/2.2/el-impl-2.2.pom)

```java
 <dependency>
	  <groupId>org.glassfish.web</groupId>
	  <artifactId>el-impl</artifactId>
	  <version>2.2</version>
     </dependency> 
```

**Updated–25-07-2012**Tomcat 7 中不再需要这个`el-ri.jar`依赖项。

## 2.JSF 2.0 托管 Bean

Java bean 或 JSF 管理的 bean，具有存储用户数据的名称属性。在 JSF，托管 bean 意味着可以从 JSF 页面访问这个 Java 类或 bean。

在 JSF 2.0 中，使用 **@ManagedBean** 注释来表示这是一个受管 Bean。HelloBean.java
T3

```java
 package com.mkyong.common;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import java.io.Serializable;

@ManagedBean
@SessionScoped
public class HelloBean implements Serializable {

	private static final long serialVersionUID = 1L;

	private String name;

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
} 
```

**Note**
In JSF 1.x, you had to declare beans in the **faces-config.xml**, but this is no longer required in JSF 2.0.

## 3.JSF 2.0 页

在 JSF 2.0 中，建议以 [XHTML 文件格式](http://web.archive.org/web/20201113070916/http://en.wikipedia.org/wiki/XHTML)创建 JSF 页面，这是一个扩展名为. XHTML 的文件。

参见下面两个 JSF 2.0 页面:

**Note**
To use the JSF 2.0 components or features, just declared the **JSF namespace** at the top of the page.

```java
 <html 
      xmlns:f="http://java.sun.com/jsf/core"      
      xmlns:h="http://java.sun.com/jsf/html"> 
```

*File:hello . XHTML*–呈现一个 JSF 文本框，并链接到“**hello Bean**”(JSF 托管 bean)、“ **name** ”属性，以及一个单击时显示“ **welcome.xhtml** ”页面的按钮。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html 
      xmlns:f="http://java.sun.com/jsf/core"      
      xmlns:h="http://java.sun.com/jsf/html">

    <h:head>
        <title>JSF 2.0 Hello World</title>
    </h:head>
    <h:body>
    	<h2>JSF 2.0 Hello World Example - hello.xhtml</h2>
    	<h:form>
    	   <h:inputText value="#{helloBean.name}"></h:inputText>
    	   <h:commandButton value="Welcome Me" action="welcome"></h:commandButton>
    	</h:form>
    </h:body>
</html> 
```

**Note**
In JSF 1.x, you had to declare the “**navigation rule**” in “**faces-config.xml**“, to tell which page to display when the button is clicked. In JSF 2.0, you can put the page name directly in the button’s “**action**” attribute. For simple navigation, it’s more than enough, but, for complex navigation, you are still advised to use the “**navigation rule**” in “**faces-config.xml**“.

*File:welcome . XHTML*–显示提交的文本框值。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html     
      xmlns:h="http://java.sun.com/jsf/html">

    <h:head>
    	<title>JSF 2.0 Hello World</title>
    </h:head>
    <h:body bgcolor="white">
    	<h2>JSF 2.0 Hello World Example - welcome.xhtml</h2>
    	<h2>Welcome #{helloBean.name}</h2>
    </h:body>
</html> 
```

**#{…}** 表示这是一个 **JSF 表达式语言**，这里是 **#{helloBean.name}** ，当页面提交时，JSF 会找到“ **helloBean** ，并通过 **setName()** 方法设置提交的 textbox 值。当显示 **welcome.xhtml** 页面时，JSF 将再次找到同一个会话“ **helloBean** ”，并通过 **getName()** 方法显示 name 属性值。

## 4.JSF 2.0 Serlvet 配置

就像任何其他标准的 web 框架一样，你需要在 **web.xml** 文件中配置 JSF 的东西。

*文件:web.xml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

        xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	id="WebApp_ID" version="2.5">

	<display-name>JavaServerFaces</display-name>

	<!-- Change to "Production" when you are ready to deploy -->
	<context-param>
		<param-name>javax.faces.PROJECT_STAGE</param-name>
		<param-value>Development</param-value>
	</context-param>

	<!-- Welcome page -->
	<welcome-file-list>
		<welcome-file>faces/hello.xhtml</welcome-file>
	</welcome-file-list>

	<!-- JSF mapping -->
	<servlet>
		<servlet-name>Faces Servlet</servlet-name>
		<servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<!-- Map these files with JSF -->
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

定义一个"**javax . faces . web app . faces servlet**"映射，映射到那些众所周知的 JSF 文件扩展名( **/faces/*** 、 ***)。jsf** ， ***。xhtml** ， ***。面相**。

在这种情况下，下面 4 个 URL 指向同一个 **hello.xhtml** 。

1.  http://localhost:8080/Java server faces/hello . JSF
2.  http://localhost:8080/Java server faces/hello . faces
3.  http://localhost:8080/Java server faces/hello . XHTML
4.  http://localhost:8080/Java server faces/faces/hello . JSF

在 JSF 2.0 开发中，建议将“**javax . faces . project _ STAGE**”设置为“**开发**”，它会提供很多有用的调试信息，让你轻松跟踪 bug。对于部署，只需将其更改为“**生产**”，您只是不想让您的客户看到这些烦人的调试信息:)。

## 5.演示

一篇长文章以一个项目演示结束🙂

*URL:http://localhost:8080/Java server faces/hello . JSF*



![jsf2-hello-world-example-1](img/3ebf8f711221fbf1a02d6c1a026f88c8.png "jsf2-hello-world-example-1")

一个简单的 JSF 页面，有一个文本框和一个按钮。



![jsf2-hello-world-example-2](img/4d05b9d1ab6b489ba5d8aa2553f9dd94.png "jsf2-hello-world-example-2")

单击按钮时，显示提交的文本框值。

## 下载源代码

Download It (v2.1.7 example)- [JSF2.0-hello-world-example-2.1.7.zip](http://web.archive.org/web/20201113070916/http://www.mkyong.com/wp-content/uploads/2010/09/JSF2.0-hello-world-example-2.1.7.zip) (8KB)Download It (old v2.1.0-b03 example)- [JSF-2-Hello-World-Example-2.1.0-b03.zip](http://web.archive.org/web/20201113070916/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-Hello-World-Example.zip) (8KB)

## 参考

1.  [JavaServer Faces 技术](http://web.archive.org/web/20201113070916/http://www.oracle.com/technetwork/java/javaee/javaserverfaces-139869.html)
2.  [JSF 2.0 发行说明](http://web.archive.org/web/20201113070916/https://javaserverfaces.java.net/nonav/rlnotes/2.0.0/releasenotes.html)
3.  Wiki : JavaServer Faces
4.  [Wiki : XHTML 文件解释](http://web.archive.org/web/20201113070916/http://en.wikipedia.org/wiki/XHTML)
5.  [Java . lang . illegalargumentexception:javax . faces . context . exception handler factory](http://web.archive.org/web/20201113070916/http://www.mkyong.com/jsf2/java-lang-illegalargumentexception-javax-faces-context-exceptionhandlerfactory/)
6.  [JSF 2.0 + Tomcat:看来这个容器的 JSP 版本比 2.1 还要老…](http://web.archive.org/web/20201113070916/http://www.mkyong.com/jsf2/jsf-2-0-tomcat-it-appears-the-jsp-version-of-the-container-is-older-than-2-1/)
7.  [Eclipse IDE:编辑器中不支持的内容类型](http://web.archive.org/web/20201113070916/http://www.mkyong.com/eclipse/eclipse-ide-unsupported-content-type-in-editor/)
8.  Eclipse IDE:。xhtml 代码辅助对 JSF 标签不起作用

Tags : [hello world](http://web.archive.org/web/20201113070916/https://mkyong.com/tag/hello-world/) [jsf2](http://web.archive.org/web/20201113070916/https://mkyong.com/tag/jsf2/)<input type="hidden" id="mkyong-current-postId" value="6986">

### 相关文章

*   [JSF 2.0 + Ajax hello world 示例](/web/20201113070916/https://mkyong.com/jsf2/jsf-2-0-ajax-hello-world-example/)
*   [PrimeFaces hello world 示例](/web/20201113070916/https://mkyong.com/jsf2/primefaces/primefaces-hello-world-example/)
*   [JAXB hello world 示例](/web/20201113070916/https://mkyong.com/java/jaxb-hello-world-example/)
*   [jsoup HTML 解析器 hello world 示例](/web/20201113070916/https://mkyong.com/java/jsoup-html-parser-hello-world-examples/)
*   [Html 教程 Hello World](/web/20201113070916/https://mkyong.com/html/html-tutorial-hello-world/)

*   [jQuery Hello World](/web/20201113070916/https://mkyong.com/jquery/jquery-hello-world/)
*   [Ant - Spring MVC 和 WAR 文件示例](/web/20201113070916/https://mkyong.com/ant/ant-spring-mvc-and-war-file-example/)
*   [SWT Hello World 示例](/web/20201113070916/https://mkyong.com/swt/swt-hello-world-example/)
*   [Wicket Hello World 示例](/web/20201113070916/https://mkyong.com/wicket/wicket-hello-world-example-with-maven-tutorial/)
*   [Maven + Spring hello world 示例](/web/20201113070916/https://mkyong.com/spring/quick-start-maven-spring-example/)