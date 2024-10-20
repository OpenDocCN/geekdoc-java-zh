# Struts 错误–NoSuchMethodError:digester . parse(Ljava/net/URL；)Ljava/lang/Object

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-error-nosuchmethoderror-digester-parseljavaneturlljavalangobject/>

## 问题

在 Struts 初始化期间，它会遇到以下错误消息。

```java
 SEVERE: Unable to initialize Struts ActionServlet due to an unexpected exception or error thrown, 
so marking the servlet as unavailable.  Most likely, this is due to an incorrect or missing library dependency.
java.lang.NoSuchMethodError: org.apache.commons.digester.Digester.parse(Ljava/net/URL;)Ljava/lang/Object;
	at org.apache.commons.chain.config.ConfigParser.parse(ConfigParser.java:190)
	at org.apache.struts.action.ActionServlet.initChain(ActionServlet.java:1687) 
```

 ## 解决办法

这是由于某些方法在 commons 消化器库中找不到。您需要将您的 **commons-digester.jar** 升级到最新版本。

在这种情况下，我使用的是“Struts 1 . 3 . 10+commons-digester-1.6 . jar+Struts-tiles 1 . 3 . 10”组合，并点击上面的错误消息。升级到最新的 **commons-digester-2.0.jar** 后，错误信息消失了。

[struts](http://web.archive.org/web/20190209023720/http://www.mkyong.com/tag/struts/)![](img/6bd88a5ae01c422c98f6019886b48539.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190209023720/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4810">







