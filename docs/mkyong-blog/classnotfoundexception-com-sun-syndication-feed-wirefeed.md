# ClassNotFoundException:com . sun . syndication . feed . wire feed

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/classnotfoundexception-com-sun-syndication-feed-wirefeed/>

## 问题

用 Spring MVC 开发 RSS，扩展了“`AbstractRssFeedView`”，在应用程序启动时出现以下错误信息。

```java
 Caused by: java.lang.NoClassDefFoundError: com/sun/syndication/feed/WireFeed
	at java.lang.Class.getDeclaredConstructors0(Native Method)
	at java.lang.Class.privateGetDeclaredConstructors(Class.java:2389)
	at java.lang.Class.getDeclaredConstructors(Class.java:1836)
	//...
Caused by: java.lang.ClassNotFoundException: com.sun.syndication.feed.WireFeed
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1516)
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1361)
	at java.lang.ClassLoader.loadClassInternal(ClassLoader.java:320)
	... 41 more 
```

 ## 解决办法

Spring MVC 使用"[罗马](http://web.archive.org/web/20190225093251/http://java.net/projects/rome/)"来生成 RSS 提要。对于 Maven，在`pom.xml`文件中包含下面的依赖关系。

```java
 <dependency>
		<groupId>net.java.dev.rome</groupId>
		<artifactId>rome</artifactId>
		<version>1.0.0</version>
	</dependency> 
```

[spring mvc](http://web.archive.org/web/20190225093251/http://www.mkyong.com/tag/spring-mvc/)![](img/969687f39547cd47c77ce11e3527f5d5.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225093251/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="9741">







