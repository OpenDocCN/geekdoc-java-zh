# Struts 2 文本标签示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-text-tag-example/>

Download It – [Struts2-Text-Tag-Example.zip](http://web.archive.org/web/20190304032014/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-Text-Tag-Example.zip)

Struts 2 " **text** "标签用于从与 action 类捆绑在一起的资源包中获取消息。并遵循三个顺序:

1.  显示资源包中的消息，遵循 [Struts 2 资源包搜索顺序](http://web.archive.org/web/20190304032014/http://www.mkyong.com/struts2/struts-2-resource-bundle-example/)。
2.  如果在资源包中没有找到消息，那么将显示标记的主体。
3.  如果标记体为空，那么将显示<text>标记中“name”属性的值。</text>

一个完整的例子很好地说明了这一点:

## 1.行动

转发请求的操作类。

**文本标签 Action.java**

```java
 package com.mkyong.common.action;

import com.opensymphony.xwork2.ActionSupport;

public class TextTagAction extends ActionSupport{

	public String execute() throws Exception {

		return SUCCESS;
	}
} 
```

 ## 2.属性文件

一个简单的属性文件，包含两个关键字“ **name.msg** 和“ **name.msg.param** ”。

**texttagaction . properties**

```java
 name.msg = "This is a message from properties file"
name.msg.param = "This is a message from properties file - param : {0}" 
```

 ## 3.文本标签示例

它显示了“文本”标签的使用。

**text.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
</head>

<body>
<h1>Struts 2 text tag example</h1>

<h2>1.<s:text name="name.msg" /></h2> 
Output : <s:text name="name.msg" />

<h2>2\. <s:text name="name.msg.unknow">message doesn't exists</s:text></h2> 
Output : <s:text name="name.msg.unknow">message doesn't exists</s:text>

<h2>3\. <s:text name="name.msg.unknow" /></h2> 
Output : <s:text name="name.msg.unknow" />

<h2>4\. <s:text name="name.msg.param" ><s:param >mkyong</s:param>
</s:text></h2> 
Output :
<s:text name="name.msg.param" >
	<s:param >mkyong</s:param>
</s:text>

</body>
</html> 
```

它是如何工作的？
**1。s:text name = " name . msg "/>**
从与当前操作类(TextTagAction.action)关联的资源包(text tag action . properties)中获取并显示消息。

```java
 "This is a message from properties file" 
```

**2。<s:text name = " name . msg . unknow ">消息不存在< /s:text >**
在资源包“text tag action . properties”或任何搜索顺序中找不到关键字，因此显示标记体。

```java
 message doesn't exists 
```

**3。<s:text name = " name . msg . unknow "/>**

```java
 name.msg.unknow 
```

**4。<s:text name = " name . msg . param ">s:param>mkyong</s:param></s:text>**
通过< param >标签将参数传入资源包。

```java
 "This is a message from properties file - param : mkyong" 
```

## 4.struts.xml

链接一下~

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
 	<constant name="struts.devMode" value="true" />
	<package name="default" namespace="/" extends="struts-default">

		<action name="textTagAction" 
			class="com.mkyong.common.action.TextTagAction" >
			<result name="success">pages/text.jsp</result>
		</action>

	</package>
</struts> 
```

## 5.演示

*http://localhost:8080/struts 2 example/text tag action . action*

**输出**

![Struts 2 text tag example](img/262d5110c01e11158ee6879dde7f9be3.png "Struts2-Text-Tag-Example")

## 参考

1.  [Struts 2 文本标签文档](http://web.archive.org/web/20190304032014/http://struts.apache.org/2.0.14/docs/text.html)

[struts2](http://web.archive.org/web/20190304032014/http://www.mkyong.com/tag/struts2/)







