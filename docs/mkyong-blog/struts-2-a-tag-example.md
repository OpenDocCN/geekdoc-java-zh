# Struts 2 标签示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-a-tag-example/>

Download It – [Struts2-A-Tag-Example.zip](http://web.archive.org/web/20190304031358/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-A-Tag-Example.zip)

Struts 2 " **一个**"标签用于呈现一个 HTML " **<一个>** "标签。最佳实践是始终使用“ **< s:url >** ”标签来创建 url 并将其嵌入到“ **a** 标签中。举个例子，

```java
 <s:url value="http://www.google.com" var="googleURL" />
<s:a href="%{googleURL}">Google</s:a> 
```

在本教程中，它展示了使用 Struts 2 " **a** 标签的 3 种方法。

## 1.行动

转发请求的操作类。

**ATagAction.java**

```java
 package com.mkyong.common.action;

import com.opensymphony.xwork2.ActionSupport;

public class ATagAction extends ActionSupport{

	public String execute() {
		return SUCCESS;
	}

} 
```

 ## 2.标签示例

一个 JSP 页面，展示了使用“ **a** 标签以不同的方式呈现 URL。

**a.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
 <html>
<head>
</head>

<body>
<h1>Struts 2 a tag example</h1>

<ol>

<li>
<s:url value="http://www.mkyong.com" var="mkyongURL" />
<s:a href="%{mkyongURL}">J2EE web development tutorials</s:a>
</li>

<li>
<s:a href="http://www.google.com">Google search engine</s:a>
</li>

<li>
<s:url action="aTagAction.action" var="aURL" />
<s:a href="%{aURL}">aTagAction</s:a>
</li>

</ol>

</body>
</html> 
```

 ## 3.struts.xml

链接一下~

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
 	<constant name="struts.devMode" value="true" />
	<package name="default" namespace="/" extends="struts-default">

		<action name="aTagAction" 
			class="com.mkyong.common.action.aTagAction" >
			<result name="success">pages/a.jsp</result>
		</action>

	</package>
</struts> 
```

## 4.演示

*http://localhost:8080/struts 2 example/atag action . action*

**输出**

![Struts 2 a tag example](img/9b82b55b9ccacb2e25c104a4428d82e9.png "Struts2-A-Tag-Example")

**在 HTML 源文件中输出**

```java
 <html> 
<head> 
</head> 

<body> 
<h1>Struts 2 a tag example</h1> 

<ol> 
<li> 
<a href="http://www.mkyong.com">J2EE web development tutorials</a> 
</li> 

<li> 
<a href="http://www.google.com">Google search engine</a> 
</li> 

<li> 
<a href="/Struts2Example/aTagAction.action">aTagAction</a> 
</li> 
</ol> 

</body> 
</html> 
```

## 参考

1.  [Struts 2 a 标签文档](http://web.archive.org/web/20190304031358/http://struts.apache.org/2.0.14/docs/a.html)

[struts2](http://web.archive.org/web/20190304031358/http://www.mkyong.com/tag/struts2/)







