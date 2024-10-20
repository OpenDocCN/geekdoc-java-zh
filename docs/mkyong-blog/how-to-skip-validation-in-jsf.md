# 如何在 JSF 跳过验证

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-skip-validation-in-jsf/>

## 问题

参见下面 JSF 的例子:

```java
 <h:inputSecret id="password" value="#{user.password}" 
		size="20" required="true" label="Password">	
		<f:validateRegex pattern="((?=.*\d).{6,20})" />
	</h:inputSecret>

	<h:message for="password" style="color:red" />

	<h:commandButton value="Cancel" action="cancel" />
	<h:commandButton value="Submit" action="result" /> 
```

如果您单击“取消”按钮，密码验证将被触发，并阻止您继续进入取消页面，这是没有意义的。有没有办法绕过 JSF 的验证？

 ## 解决办法

要跳过验证，向 cancel 按钮添加一个 **immediate="true"** 属性。

```java
 <h:inputSecret id="password" value="#{user.password}" 
		size="20" required="true" label="Password">			
	</h:inputSecret>

	<h:message for="password" style="color:red" />

	<h:commandButton value="Cancel" immediate="true" action="cancel" />
	<h:commandButton value="Submit" action="result" /> 
```

[jsf2](http://web.archive.org/web/20190116132727/http://www.mkyong.com/tag/jsf2/) [validation](http://web.archive.org/web/20190116132727/http://www.mkyong.com/tag/validation/)![](img/1355d307ad31f424033effb012051516.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190116132727/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="7534">







