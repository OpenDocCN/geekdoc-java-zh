# 在 org . Apache . struts . action . message 项下找不到消息资源

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/cannot-find-message-resources-under-key-org-apache-struts-action-message/>

## 问题

Struts 框架中一个常见的资源包错误，它通常是由系统找不到相应的消息资源引起的。

```java
 javax.servlet.jsp.JspException: Cannot find message resources under key org.apache.struts.action.MESSAGE
	org.apache.struts.taglib.TagUtils.retrieveMessageResources(TagUtils.java:1112)
	org.apache.struts.taglib.TagUtils.present(TagUtils.java:1055)
	org.apache.struts.taglib.html.ErrorsTag.doStartTag(ErrorsTag.java:200)
	org.apache.jsp.pages.login_jsp._jspx_meth_html_005ferrors_005f0(login_jsp.java:160)
	org.apache.jsp.pages.login_jsp._jspx_meth_html_005fform_005f0(login_jsp.java:111)
	org.apache.jsp.pages.login_jsp._jspService(login_jsp.java:77)
	org.apache.jasper.runtime.HttpJspBase.service(HttpJspBase.java:70)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:831)
	org.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:377)
	org.apache.jasper.servlet.JspServlet.serviceJspFile(JspServlet.java:313)
	org.apache.jasper.servlet.JspServlet.service(JspServlet.java:260)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:831)
	org.apache.struts.chain.commands.servlet.PerformForward.handleAsForward(PerformForward.java:113)
	org.apache.struts.chain.commands.servlet.PerformForward.perform(PerformForward.java:96)
	org.apache.struts.chain.commands.AbstractPerformForward.execute(AbstractPerformForward.java:54)
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

只需包含相应的消息资源。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<message-resources
		parameter="com.mkyong.common.properties.Common" />

</struts-config> 
```

[struts](http://web.archive.org/web/20190209024201/http://www.mkyong.com/tag/struts/)![](img/0e8d920198f70edf11ddf73b778a096b.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190209024201/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4470">







