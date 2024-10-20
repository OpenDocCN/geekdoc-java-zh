# Struts 2 execAndWait 拦截器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-execandwait-interceptor-example/>

Download it – [Struts2-ExecAndWait-Interceptor-Example.zip](http://web.archive.org/web/20190214232659/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-ExecAndWait-Interceptor-Example.zip)

Struts 2 附带了一个非常有趣的“**执行和等待**”拦截器，名为“**执行和等待**”，这是一个非常方便的拦截器，用于在后台长时间运行的操作，同时向用户显示一个自定义的等待页面。在本教程中，它展示了一个使用 Struts 2 **execAndWait** 拦截器的完整示例。

## 1.行动

一个普通的 action 类，用一个很长的运行过程来演示 **execAndWait** 效果。

**LongProcessAction.java**

```java
 package com.mkyong.common.action;

import com.opensymphony.xwork2.ActionSupport;

public class LongProcessAction extends ActionSupport{

	public String execute() throws Exception {

		//it should be delay few seconds, 
		//unless you have a super powerful computer.
		for(int i =0; i<1000000; i++){
			System.out.println(i);
		}
		return SUCCESS;

	}
} 
```

 ## 2.JSP 页面

创建两个页面:

1.  **wait.jsp**-在处理长时间运行的流程时向用户显示。
2.  **success.jsp**——流程完成后显示给用户。

**HTML meta refresh**
Remember to put the meta refresh on top of the waiting page; Otherwise, the page will not redirect to the success page, even the process is completed.

在这个**wait.jsp**中，元刷新被设置为每 5 秒重新加载一次页面，如果该过程完成，它将重定向到 success.jsp 的**，否则停留在同一页面。**

**wait.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
<meta http-equiv="refresh" content="5;url=<s:url includeParams="all" />"/>
</head>

<body>
<h1>Struts 2 execAndWait example</h1>

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>Please wait while we process your request...</h2>

</body>
</html> 
```

**success.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
</head>

<body>
<h1>Struts 2 execAndWait example</h1>

<h2>Done</h2>

</body>
</html> 
```

## 3.执行和等待拦截器

链接 action 类并声明了" **execAndWait** "拦截器。

**execAndWait parameters**

1.  **延迟(可选)**:显示 wait.jsp 的初始延迟时间，以毫秒为单位。默认为无延迟。
2.  **delaySleepInterval(可选)**:检查后台进程是否已经完成的间隔，以毫秒为单位。默认值为 100 毫秒。

**struts.xml**

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
 	<constant name="struts.devMode" value="true" />

	<package name="default" namespace="/" extends="struts-default">

		<action name="longProcessAction" 
			class="com.mkyong.common.action.LongProcessAction" >

			<interceptor-ref name="execAndWait">
		        <param name="delay">1000</param>
		        <param name="delaySleepInterval">500</param>
		    </interceptor-ref>

		    <result name="wait">pages/wait.jsp</result>
		    <result name="success">pages/success.jsp</result>
		</action>

	</package>

</struts> 
```

在这种情况下，它将延迟 1 秒显示**wait.jsp**，并每隔 500 毫秒检查一次后台进程是否已经完成。即使这个过程完成了，仍然需要等待 wait.jsp 的**meta refresh 来激活页面重载。**

## 4.演示

访问 URL:*http://localhost:8080/struts 2 example/longprocessaction . action*

延时 1 秒，显示**wait.jsp**。

![Struts 2 ExecAndWait interceptor example](img/3db94035975fa7f26b5f7b0b53a1852e.png "Struts2-ExecAndWait-Interceptor-Example1")

过程完成后，自动显示**success.jsp**。

![Struts 2 ExecAndWait interceptor example](img/e5fd5d38170a4fe99dd9272c011aa287.png "Struts2-ExecAndWait-Interceptor-Example2")

## 参考

1.  [Struts 2 execAndWait 拦截器文档](http://web.archive.org/web/20190214232659/http://struts.apache.org/2.1.8/docs/execute-and-wait-interceptor.html)
2.  [HTML 元刷新](http://web.archive.org/web/20190214232659/http://en.wikipedia.org/wiki/Meta_refresh)

[interceptor](http://web.archive.org/web/20190214232659/http://www.mkyong.com/tag/interceptor/) [struts2](http://web.archive.org/web/20190214232659/http://www.mkyong.com/tag/struts2/)![](img/00d6531c0b6dcaf856a30054c1e3d4f8.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214232659/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6315">







