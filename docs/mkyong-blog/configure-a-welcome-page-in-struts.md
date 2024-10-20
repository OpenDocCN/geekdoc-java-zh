# 在 Struts 中配置欢迎页面

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/configure-a-welcome-page-in-struts/>

每个网站都需要一个欢迎页面或默认页面作为入口点。这里有 3 种在 Struts 中配置欢迎页面的方法。

Download this Struts welcome file example – [Struts-Welcome-File-Example.zip](http://web.archive.org/web/20190225095125/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-Welcome-File-Example.zip)

## 1.index.jsp

最简单的方法是创建一个“**index.jsp**页面，并把它与 **WEB-INF** 文件夹，项目根文件夹放在同一层。

访问项目根目录

```java
 http://localhost:8080/StrutsExample/ 
```

它将默认为 index.jsp 内部。

```java
 http://localhost:8080/StrutsExample/index.jsp 
```

 ## 2.web.xml 欢迎文件

在 web.xml 文件中声明一个**欢迎文件**。

```java
 <welcome-file-list>
	<welcome-file>
		/pages/Welcome.jsp
	</welcome-file>
  </welcome-file-list> 
```

访问项目根目录

```java
 http://localhost:8080/StrutsExample/ 
```

它将在内部重定向到 welcome.jsp 文件。

```java
 http://localhost:8080/StrutsExample/pages/Welcome.jsp 
```

*web.xml*

```java
 <!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Maven Struts Examples</display-name>

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

  <welcome-file-list>
	<welcome-file>
		/pages/Welcome.jsp
	</welcome-file>
  </welcome-file-list>

</web-app> 
```

 ## 3.JSP 转发

如方法 1 所述，创建一个“**index.jsp**”文件，并定义一个 JSP forward 标记，将其重定向到另一个 Struts 动作。

*index.jsp*

声明一个 **/Welcome** web 路径，用一个 **ForwardAction** 类型将其转发到另一个 JSP 文件。

*struts-config.xml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<action-mappings>

		<action
			path="/Welcome"
			type="org.apache.struts.actions.ForwardAction"
			parameter="/pages/Welcome.jsp"/>

	</action-mappings>

</struts-config> 
```

[struts](http://web.archive.org/web/20190225095125/http://www.mkyong.com/tag/struts/) [welcome page](http://web.archive.org/web/20190225095125/http://www.mkyong.com/tag/welcome-page/)







