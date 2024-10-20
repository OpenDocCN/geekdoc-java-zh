# Struts 2 覆盖了拦截器参数

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-override-the-interceptor-parameters/>

在 Struts 2 中，您可以通过通用的 **< param >** 标记来设置或覆盖拦截器参数。参见下面的例子

```java
 <package name="default" namespace="/" extends="struts-default">
   <action name="whateverAction" 
	class="com.mkyong.common.action.WhateverAction" >
	<interceptor-ref name="workflow">
		<param name="excludeMethods">whateverMethod</param>
	</interceptor-ref>
	<result name="success">pages/whatever.jsp</result>
   </action>		
</package> 
```

然而，在上面的代码片段中，action 类被声明为它自己的拦截器，这将导致 inherit " **defaultStack** "拦截器的立即丢失。

如果您想保留" **defaultStack** "拦截器，并覆盖**工作流的 excludeMethods** 参数，该怎么办？没问题，试试这个

```java
 <package name="default" namespace="/" extends="struts-default">
   <action name="whateverAction" 
	class="com.mkyong.common.action.WhateverAction" >
	<interceptor-ref name="defaultStack">
		<param name="workflow.excludeMethods">whateverMethod</param>
	</interceptor-ref>
	<result name="success">pages/whatever.jsp</result>
   </action>		
</package> 
```

上面的代码片段将保留“ **defaultStack** ”拦截器，并覆盖“**工作流**”参数。

## 参考

1.  [Struts 2 拦截器文档](http://web.archive.org/web/20190214225208/http://struts.apache.org/2.1.8/docs/interceptors.html)
2.  [Struts 2 工作流拦截器文档](http://web.archive.org/web/20190214225208/http://struts.apache.org/2.0.14/docs/workflow-interceptor.html)

[interceptor](http://web.archive.org/web/20190214225208/http://www.mkyong.com/tag/interceptor/) [struts2](http://web.archive.org/web/20190214225208/http://www.mkyong.com/tag/struts2/)![](img/63ea8ff3b5f0b71c722a87e04ae85ffb.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214225208/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6262">







