# Struts 2 设置标签示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-set-tag-example/>

Download It – [Struts2-Set-Tag-Example.zip](http://web.archive.org/web/20190304030901/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-Set-Tag-Example.zip)

Struts 2 " **set** 标签用于在指定的作用域(应用、会话、请求、页面或动作)中给变量赋值，动作是默认的作用域。参见一个完整的“**设置**标记示例:

The “**value**” means any hard-coded String, property value or just anything you can reference.

## 1.行动

带有“msg”属性的操作类。

**SetTagAction.java**

```java
 package com.mkyong.common.action;

import com.opensymphony.xwork2.ActionSupport;

public class SetTagAction extends ActionSupport{

	private String msg = "Struts 2 is a funny framework";

	public String getMsg() {
		return msg;
	}

	public String execute() throws Exception {

		return SUCCESS;
	}
} 
```

 ## 2.设置标签示例

它显示了使用“ **set** 标签。

**set.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
</head>

<body>
<h1>Struts 2 set tag example</h1>
<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>1\. <s:set var="varMsg" value="msg" /></h2>

<s:set var="varMsg" value="msg" />
<s:property value="varMsg" />

<h2>2\. <s:set var="varUrl" value="%{'http://www.mkyong.com'}" /></h2> 

<s:set var="varUrl" value="%{'http://www.mkyong.com'}" />
<s:property value="varUrl" />

</body>
</html> 
```

它是如何工作的？

**1。<s:set var = " varMsg " value = " msg "/>**
调用动作的 **getMsg()** 方法，将返回值赋给一个名为" **varMsg** "的变量。

**2。<s:set var = " varUrl " value = " % { ' http://www . mkyong . com ' } "/>**
硬编码一个字符串，赋给一个名为" **varUrl** 的变量。

**Assign value to a variable, not property value.**

举个例子，

```java
 public class SetTagAction extends ActionSupport{

	private String msg;

	public String setMsg(String msg) {
		this.msg = msg;
	}
	... 
```

```java
 <s:set var="msg" value="%{'this is a message'}" /> 
```

许多 Struts 2 开发人员认为" **set** 标签 **var="msg"** 会通过 **setMsg()** 方法将值赋给关联的 action 类。

**这是错误的**，set 标签不会调用 **setMsg()** 方法，它只会将“值”赋给一个名为“ **msg** 的变量，而不是动作的属性值。

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

		<action name="setTagAction" 
			class="com.mkyong.common.action.SetTagAction" >
			<result name="success">pages/set.jsp</result>
		</action>

	</package>
</struts> 
```

## 5.演示

*http://localhost:8080/struts 2 example/setta action . action*

**输出**

![Struts 2 set tag example](img/772743a6d423213507ad716e06ad5280.png "Struts2-Set-Tag-Example")

## 参考

1.  [Struts 2 设置标签文档](http://web.archive.org/web/20190304030901/http://struts.apache.org/2.1.8/docs/set.html)

[struts2](http://web.archive.org/web/20190304030901/http://www.mkyong.com/tag/struts2/)![](img/7b0b1e46b8fc2dfcd50aed95a372278d.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304030901/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6234">







