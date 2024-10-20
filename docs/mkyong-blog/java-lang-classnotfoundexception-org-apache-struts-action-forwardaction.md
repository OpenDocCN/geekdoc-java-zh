# Java . lang . classnotfoundexception:org . Apache . struts . action . forward action

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/java-lang-classnotfoundexception-org-apache-struts-action-forwardaction/>

## 问题

一个超级常见的 Struts 错误消息，找不到**org . Apache . Struts . action . forward action**类。

```java
 javax.servlet.ServletException: java.lang.ClassNotFoundException: org.apache.struts.action.ForwardAction
	org.apache.struts.chain.ComposableRequestProcessor.process(ComposableRequestProcessor.java:286)
	org.apache.struts.action.ActionServlet.process(ActionServlet.java:1913)
	org.apache.struts.action.ActionServlet.doGet(ActionServlet.java:449)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:718)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:831)

root cause

java.lang.ClassNotFoundException: org.apache.struts.action.ForwardAction
	org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1516)
	org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1361)
	org.apache.struts.chain.commands.util.ClassUtils.getApplicationClass(ClassUtils.java:54)
	org.apache.struts.chain.commands.util.ClassUtils.getApplicationInstance(ClassUtils.java:71)
	org.apache.struts.chain.commands.servlet.CreateAction.createAction(CreateAction.java:98)
	org.apache.struts.chain.commands.servlet.CreateAction.getAction(CreateAction.java:68)
	org.apache.struts.chain.commands.AbstractCreateAction.execute(AbstractCreateAction.java:91)
	org.apache.struts.chain.commands.ActionCommandBase.execute(ActionCommandBase.java:51)
	org.apache.commons.chain.impl.ChainBase.execute(ChainBase.java:191)
	org.apache.commons.chain.generic.LookupCommand.execute(LookupCommand.java:305)
	org.apache.commons.chain.impl.ChainBase.execute(ChainBase.java:191)
	org.apache.struts.chain.ComposableRequestProcessor.process(ComposableRequestProcessor.java:283)
	org.apache.struts.action.ActionServlet.process(ActionServlet.java:1913)
	org.apache.struts.action.ActionServlet.doGet(ActionServlet.java:449)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:718)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:831) 
```

 ## 解决办法

ForwardAction 类打包在**org . Apache . struts . actions . forward action**中，而不是**org . Apache . struts . action . forward action**中，后面带“s”的动作。

[struts](http://web.archive.org/web/20190308095632/http://www.mkyong.com/tag/struts/)![](img/c8bfc21607327b61a003071df2a50ce9.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190308095632/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4448">







