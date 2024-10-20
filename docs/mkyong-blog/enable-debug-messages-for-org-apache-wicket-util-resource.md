# 为 org . Apache . wicket . util . resource 启用调试消息

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/enable-debug-messages-for-org-apache-wicket-util-resource/>

## 问题

在 Wicket 中，当一个 html 页面没有被找到时，它将为 org . Apache . Wicket . util . resource 发出" **Enable debug messages，以获得所有被尝试的文件名的列表**。想知道如何为 Wicket 资源启用调试消息吗？

```java
 Root cause:
org.apache.wicket.markup.MarkupNotFoundException: 
Markup of type 'html' for component 'com.mkyong.hello.Hello' not found. 

Enable debug messages for org.apache.wicket.util.resource to get a list of all filenames tried.: 
[Page class = com.mkyong.hello.Hello, id = 0, version = 0]
at org.apache.wicket.markup.MarkupCache.getMarkupStream(MarkupCache.java:227)
... 
```

 ## 解决办法

我不知道如何在“`org.apache.wicket.util.resource`”上启用调试，Wicket 错误消息应该更清楚！

或者，您可以在调试模式下启用日志记录，并通过日志文件跟踪 Wicket 如何找到资源。例如，[集成 log4j 和 Wicket](http://web.archive.org/web/20190225092432/http://www.mkyong.com/wicket/wicket-log4j-integration-example/) 。

*log4j 输出的示例…*

```java
 DEBUG MarkupCache:300 - Load markup: cacheKey=com.mkyong.hello.Helloen_US.html
DEBUG ResourceStreamLocator:216 - Attempting to locate resource 
'com/mkyong/hello/Hello_en_US.html' on path [folders = [], webapppaths: [/pages/]]

DEBUG ResourceStreamLocator:186 - Attempting to locate resource 
'com/mkyong/hello/Hello_en_US.html' using classloader WebappClassLoader
  delegate: false
  repositories:
    /WEB-INF/classes/
...
DEBUG ResourceStreamLocator:216 - Attempting to locate resource 
'com/mkyong/hello/Hello.html' on path [folders = [], webapppaths: [/pages/]]
DEBUG ResourceStreamLocator:186 - Attempting to locate resource 
'com/mkyong/hello/Hello.html' using classloader WebappClassLoader
  delegate: false
  repositories:
    /WEB-INF/classes/
... 
```

[wicket](http://web.archive.org/web/20190225092432/http://www.mkyong.com/tag/wicket/)![](img/64038b235f68c7f514d324fbcc9d0d76.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225092432/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8942">







