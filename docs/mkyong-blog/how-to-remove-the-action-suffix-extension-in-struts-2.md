# 如何删除 Struts 2 中的 action 后缀扩展

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/how-to-remove-the-action-suffix-extension-in-struts-2/>

Download It – [Struts2-Custom-Extension-Example.zip](http://web.archive.org/web/20190303140546/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-Custom-Extension-Example.zip)

在 Struts 2 中，所有的 action 类都有一个默认的**后缀。动作扩展**。举个例子，

```java
 <struts>
  <package name="default" namespace="/" extends="struts-default">
	<action name="SayStruts2">
		<result>pages/printStruts2.jsp</result>
	</action>
  </package>
</struts> 
```

要访问“SayStruts2”操作类，请使用以下 URL:

```java
 Action URL : http://localhost:8080/Struts2Example/SayStruts2.action 
```

## 配置操作扩展

Struts 2 允许轻松配置动作扩展，要更改它，只需声明一个常量" **struts.action.extension** 值:

 ## 1.html 扩展

将操作类更改为。html 扩展。

```java
 <struts>

  <constant name="struts.action.extension" value="html"/> 

  <package name="default" namespace="/" extends="struts-default">
	<action name="SayStruts2">
		<result>pages/printStruts2.jsp</result>
	</action>
  </package>

</struts> 
```

现在，您可以通过以下方式访问“SayStruts2”操作类

```java
 Action URL : http://localhost:8080/Struts2Example/SayStruts2.html 
```

 ## 2.没有扩展

将操作类更改为空扩展。

```java
 <struts>

  <constant name="struts.action.extension" value=""/> 

  <package name="default" namespace="/" extends="struts-default">
	<action name="SayStruts2">
		<result>pages/printStruts2.jsp</result>
	</action>
  </package>

</struts> 
```

现在，您可以通过以下方式访问“SayStruts2”操作类

```java
 Action URL : http://localhost:8080/Struts2Example/SayStruts2 
```

[extension](http://web.archive.org/web/20190303140546/http://www.mkyong.com/tag/extension/) [struts2](http://web.archive.org/web/20190303140546/http://www.mkyong.com/tag/struts2/)







