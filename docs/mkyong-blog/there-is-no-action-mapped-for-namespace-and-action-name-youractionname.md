# 没有为命名空间/和操作名“yourActionName”映射的操作

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/there-is-no-action-mapped-for-namespace-and-action-name-youractionname/>

## 问题

许多 Struts 2 开发人员声称他们已经正确配置了 action 类，但是在访问 action 类时遇到了 actions 错误消息。

查看两种模式下的错误信息:

 ## 1.“struts.devMode”处于关闭状态(默认)

```java
 HTTP Status 404 - 
There is no Action mapped for namespace / and action name "yourActionName".

type Status report 

message There is no Action mapped for namespace / and action name "yourActionName".

description The requested resource 
(There is no Action mapped for namespace / and action name "yourActionName".) is not available. 
```

 ## 2.struts.devMode 已打开

在`struts.xml`

```java
 <constant name="struts.devMode" value="true" /> 
```

```java
 Struts Problem Report

Struts has detected an unhandled exception:

Messages:	
There is no Action mapped for namespace / and action name "yourActionName".
Stacktraces

There is no Action mapped for namespace / and action name "yourActionName". - [unknown location]
    com.opensymphony.xwork2.DefaultActionProxy.prepare(DefaultActionProxy.java:178)
    org.apache.struts2.impl.StrutsActionProxy.prepare(StrutsActionProxy.java:61)
   ...

You are seeing this page because development mode is enabled. Development mode, or devMode, 
enables extra debugging behaviors and reports to assist developers. To disable this mode, set:

  struts.devMode=false
in your WEB-INF/classes/struts.properties file. 
```

## 解决办法

上面的错误信息是说 action 类不可用，这意味着您在您的 action 类配置中做错了什么，可能是一个**名称空间**或**错别字**错误，只是仔细检查名称。

这里有一个工作动作类配置在`struts.xml`文件中，可能会供你参考使用。

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
 	<constant name="struts.devMode" value="true" />
	<package name="default" namespace="/" extends="struts-default">

		<action name="abcAction" 
			class="com.mkyong.common.action.AbcAction" >
			<result name="success">pages/abc.jsp</result>
		</action>

	</package>
</struts> 
```

假设项目的根上下文是" **Struts2Example** "，那么你可以通过这个 URL 访问上面的动作——
T3【http://localhost:8080/struts 2 example/ABC action . action

[struts2](http://web.archive.org/web/20190304030444/http://www.mkyong.com/tag/struts2/)







