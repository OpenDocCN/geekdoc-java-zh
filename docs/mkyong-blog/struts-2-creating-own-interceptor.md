# Struts 2 创建自己的拦截器

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-creating-own-interceptor/>

Download it – [Struts2-Create-Own-Interceptor-Example.zip](http://web.archive.org/web/20190214234608/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-Create-Own-Interceptor-Example.zip)

在本教程中，它展示了如何在 Struts 2 中创建自己的拦截器。

总结步骤:

1.  创建一个类实现**com . open symphony . xwork 2 . interceptor . interceptor**。
2.  实现 **intercept(ActionInvocation 调用)**方法。
3.  在 **struts.xml** 中配置拦截器。
4.  把它和行动联系起来。

**Struts 2 interceptors**
Struts 2 comes with many ready interceptors, make sure you check the list of the [available Struts 2 interceptors](http://web.archive.org/web/20190214234608/http://struts.apache.org/2.0.14/docs/interceptors.html) before you create your own interceptor.

创建自己的拦截器的完整示例:

## 1.行动

转发用户请求和打印消息的简单操作。

**HelloAction.java**

```java
 package com.mkyong.common.action;

import com.opensymphony.xwork2.ActionSupport;

public class HelloAction extends ActionSupport{

	public String execute() throws Exception {

		System.out.println("HelloAction execute() is called");
		return SUCCESS;

	}
} 
```

 ## 2.拦截机

一个完整的拦截器示例。

**PrintMsgInterceptor.java**

```java
 package com.mkyong.common.interceptor;

import com.opensymphony.xwork2.ActionInvocation;
import com.opensymphony.xwork2.interceptor.Interceptor;

public class PrintMsgInterceptor implements Interceptor{

        //called during interceptor destruction
	public void destroy() {
		System.out.println("CustomInterceptor destroy() is called...");
	}

	//called during interceptor initialization
	public void init() {
		System.out.println("CustomInterceptor init() is called...");
	}

	//put interceptor code here
	public String intercept(ActionInvocation invocation) throws Exception {

		System.out.println("CustomInterceptor, before invocation.invoke()...");

		String result = invocation.invoke();

		System.out.println("CustomInterceptor, after invocation.invoke()...");

		return result;
	}

} 
```

**解释**
拦截器类必须实现**com . open symphony . xwork 2 . interceptor . interceptor 接口**。拦截器初始化时，调用**init()**；拦截器破坏， **destroy()** 被调用。最后，将所有完成工作的拦截器代码放在**intercept(action invocation invocation)**方法中。

**invocation.invoke()**
In the interceptor intercept() method, you **must called the invocation.invoke()** and return it’s result. This is the method responsible for calling the next interceptor or the action. The action will failed to continue without calling the **invocation.invoke()** method.**destroy() is not reliable**
It’s not recommend to put any code inside the **destroy()**, because this method is not reliable. When your application server is force shutdown or be killed by command, the **destroy()** will not be called. ## 3.struts.xml

在 **struts.xml** 文件中配置拦截器。

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
  <package name="default" namespace="/" extends="struts-default">

    <interceptors>	
	<interceptor name="printMsgInterceptor" 
	class="com.mkyong.common.interceptor.PrintMsgInterceptor"></interceptor>

         <interceptor-stack name="newStack">
     		<interceptor-ref name="printMsgInterceptor"/>
		<interceptor-ref name="defaultStack" />
          </interceptor-stack>

    </interceptors>

    <action name="helloAction" 
	class="com.mkyong.common.action.HelloAction" >
	<interceptor-ref name="newStack"/>
	<result name="success">pages/hello.jsp</result>
     </action>

   </package>
</struts> 
```

## 4.演示

在服务器初始化期间，拦截器 **init()** 方法被调用。

```java
 INFO: Overriding property struts.i18n.reload - old value: false new value: true
15 Julai 2010 11:37:42 AM com.opensymphony.xwork2.util.logging.commons.CommonsLogger info
INFO: Overriding property struts.configuration.xml.reload - old value: false new value: true

CustomInterceptor init() is called...

15 Julai 2010 11:37:42 AM org.apache.coyote.http11.Http11Protocol start
INFO: Starting Coyote HTTP/1.1 on http-8080
15 Julai 2010 11:37:42 AM org.apache.jk.common.ChannelSocket init
INFO: JK: ajp13 listening on /0.0.0.0:8009
15 Julai 2010 11:37:42 AM org.apache.jk.server.JkMain start
INFO: Jk running ID=0 time=0/20  config=null
15 Julai 2010 11:37:42 AM org.apache.catalina.startup.Catalina start
INFO: Server startup in 994 ms 
```

当您通过 URL 访问操作时:*http://localhost:8080/struts 2 example/hello action . action*

```java
 INFO: Overriding property struts.i18n.reload - old value: false new value: true
15 Julai 2010 11:37:42 AM com.opensymphony.xwork2.util.logging.commons.CommonsLogger info
INFO: Overriding property struts.configuration.xml.reload - old value: false new value: true

CustomInterceptor init() is called...

15 Julai 2010 11:37:42 AM org.apache.coyote.http11.Http11Protocol start
INFO: Starting Coyote HTTP/1.1 on http-8080
15 Julai 2010 11:37:42 AM org.apache.jk.common.ChannelSocket init
INFO: JK: ajp13 listening on /0.0.0.0:8009
15 Julai 2010 11:37:42 AM org.apache.jk.server.JkMain start
INFO: Jk running ID=0 time=0/20  config=null
15 Julai 2010 11:37:42 AM org.apache.catalina.startup.Catalina start
INFO: Server startup in 994 ms

CustomInterceptor, before invocation.invoke()...
HelloAction execute() is called
CustomInterceptor, after invocation.invoke()... 
```

## 参考

1.  [Struts 2 拦截器文档](http://web.archive.org/web/20190214234608/http://struts.apache.org/2.1.8/docs/interceptors.html)
2.  [Struts 2 构建自己的拦截器](http://web.archive.org/web/20190214234608/http://struts.apache.org/2.0.14/docs/building-your-own-interceptor.html)

[interceptor](http://web.archive.org/web/20190214234608/http://www.mkyong.com/tag/interceptor/) [struts2](http://web.archive.org/web/20190214234608/http://www.mkyong.com/tag/struts2/)







