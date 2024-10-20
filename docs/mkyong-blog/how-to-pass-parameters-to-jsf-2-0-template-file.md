# 如何向 JSF 2.0 模板文件传递参数？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-pass-parameters-to-jsf-2-0-template-file/>

在 JSF 2.0 web 应用程序中，您可以使用" **ui:param** " facelets 标记将参数传递给包含文件或模板文件。

## 1.“ui:include”中的参数

向包含文件传递“标签行”参数的示例。

```java
 <ui:insert name="header" >
   <ui:include src="/template/common/commonHeader.xhtml">

	<ui:param name="tagLine" value="JSF a day, bug away" />

   </ui:include>
</ui:insert> 
```

在 **commonHeader.xhtml** 文件中，可以用普通的 EL 表达式访问“tagLine”参数。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:ui="http://java.sun.com/jsf/facelets"
      >
    <body>
        <ui:composition>
	    <script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>Tag Line : #{tagLine}</h2>
	</ui:composition>
    </body>
</html> 
```

 ## 2.“ui:合成”中的参数

将“curFileName”参数传递给模板文件的示例。

```java
 <ui:composition template="template/common/commonLayout.xhtml">

	<ui:param name="curFileName" value="default.xhtml" />

</ui:composition> 
```

在 **commonLayout.xhtml** 文件中，可以用普通的 EL 表达式访问“curFileName”参数。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      >
    <h:body>

	<div id="content">
	        Current File : #{curFileName}
	</div>

    </h:body>
</html> 
```

## 下载源代码

Download It – [JSF-2-Parameter-In-Template-Example.zip](http://web.archive.org/web/20190224144307/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-Parameter-In-Template-Example.zip) (12KB)

#### 参考

1.  [JSF<ui:param/>JavaDoc](http://web.archive.org/web/20190224144307/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/ui/param.html)

[JSF 2](http://web.archive.org/web/20190224144307/http://www.mkyong.com/tag/jsf2/)[参数](http://web.archive.org/web/20190224144307/http://www.mkyong.com/tag/parameter/) ![](img/f3120f167fa9a5d670c292449e1fb563.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224144307/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="7340">







