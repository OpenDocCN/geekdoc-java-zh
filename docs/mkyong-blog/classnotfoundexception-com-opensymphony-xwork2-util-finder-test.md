# notfounindexception:com . opensymphony . xwork 2 . util . finder . test

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/classnotfoundexception-com-opensymphony-xwork2-util-finder-test/>

## 问题

一个启用了 Struts 2 注释的项目，在服务器启动期间点击了以下错误消息。

```java
 Caused by: java.lang.NoClassDefFoundError: com/opensymphony/xwork2/util/finder/Test
  at java.lang.Class.getDeclaredConstructors0(Native Method)
  at java.lang.Class.privateGetDeclaredConstructors(Unknown Source)
  at java.lang.Class.getDeclaredConstructors(Unknown Source)
  ... 24 more
Caused by: java.lang.ClassNotFoundException: com.opensymphony.xwork2.util.finder.Test
  at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1516)
  at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1361)
  ... 28 more 
```

 ## 解决办法

缺少" **xwork.jar** "库，这是 Struts 2 开发中所需要的。从 Maven 中央存储库下载它。

```java
 <dependency>
           <groupId>com.opensymphony</groupId>
           <artifactId>xwork</artifactId>
           <version>2.1.3</version>
    </dependency> 
```

[struts2](http://web.archive.org/web/20190225092427/http://www.mkyong.com/tag/struts2/)![](img/f852f997d072eaf2189a7c7a4e3df075.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225092427/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5701">







