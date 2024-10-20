# Struts 2 名称空间配置示例和说明

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-namespace-configuration-example-and-explanation/>

Struts 2 名称空间是一个新概念，通过给每个模块一个名称空间来处理多个模块。此外，它可以用来避免位于不同模块的相同动作名之间的冲突。

Download It – [Struts2-NameSpace-Configuration-Example.zip](http://web.archive.org/web/20190225093132/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-NameSpace-Configuration-Example.zip)Struts 2 Namespaces are the equivalent of [Struts 1 multiple modules](http://web.archive.org/web/20190225093132/http://www.mkyong.com/struts/struts-multiple-configuration-files-example/)

请查看此图片，了解 URL 如何匹配 Struts 2 操作名称空间。

![namespace map url](img/27346d836b6a2fbd854e3d75eb786a3e.png "namespace-example")

## 1.命名空间配置

让我们通过一个 Struts 2 namescape 配置示例来了解它如何与 URL 和文件夹相匹配。

*P.S 包“名字”不会影响结果，只要给个有意义的名字就行。*

**struts.xml**

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>

<package name="default" namespace="/" extends="struts-default">
	<action name="SayWelcome">
		<result>pages/welcome.jsp</result>
	</action>
</package>

<package name="common" namespace="/common" extends="struts-default">
	<action name="SayWelcome">
		<result>pages/welcome.jsp</result>
	</action>
</package>

<package name="user" namespace="/user" extends="struts-default">
	<action name="SayWelcome">
		<result>pages/welcome.jsp</result>
	</action>
</package>

</struts> 
```

Struts 2 动作名称空间映射到文件夹结构。

![namespace map folder](img/6280ff05e5e2080ff0163d1dd70cfd3a.png "namespace-example-folder") ## 2.JSP 视图页面

3 个 JSP 视图页面具有相同的文件名，但是位于不同的模块。

**Root–web app/pages/welcome . JSP**

```java

Struts 2 名称空间示例

 ## Welcome - namespace = "root " 
```

**通用模块–web app/Common/pages/welcome . JSP**

```java

Struts 2 名称空间示例

Welcome - namespace = "常见"

```

**用户模块–web app/User/pages/welcome . JSP**

```java

Struts 2 名称空间示例

Welcome - namespace = "用户"

```

## 3.映射–它是如何工作的？

**例 1**
URL:*http://localhost:8080/struts 2 Example/say welcome . action*
将匹配根命名空间。

```java
 <package name="default" namespace="/" extends="struts-default">
	<action name="SayWelcome">
		<result>pages/welcome.jsp</result>
	</action>
</package> 
```

并显示 **webapp/pages/welcome.jsp** 的内容。

**例 2**
URL:*http://localhost:8080/struts 2 Example/common/say welcome . action*
将匹配通用名称空间。

```java
 <package name="common" namespace="/common" extends="struts-default">
	<action name="SayWelcome">
		<result>pages/welcome.jsp</result>
	</action>
</package> 
```

并显示**web app/common/pages/welcome . JSP**的内容。

**例 3**
URL:*http://localhost:8080/struts 2 Example/user/say welcome . action*
将匹配用户命名空间。

```java
 <package name="user" namespace="/user" extends="struts-default">
	<action name="SayWelcome">
		<result>pages/welcome.jsp</result>
	</action>
</package> 
```

并显示**web app/user/pages/welcome . JSP**的内容。

## 参考

1.  [Struts 2 名称空间配置参考](http://web.archive.org/web/20190225093132/http://struts.apache.org/2.0.14/docs/namespace-configuration.html)

[namespace](http://web.archive.org/web/20190225093132/http://www.mkyong.com/tag/namespace/) [struts2](http://web.archive.org/web/20190225093132/http://www.mkyong.com/tag/struts2/)![](img/8ba7484a2f1563982d5f7c3951f9a24f.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225093132/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5646">







