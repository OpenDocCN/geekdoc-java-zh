# ClassNotFoundException:com . thoughtworks . xstream . io . hierarchical streamreader

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/classnotfoundexception-com-thoughtworks-xstream-io-hierarchicalstreamreader/>

## 问题

Spring MVC 中的 XML 开发，通过 oxm，命中“`HierarchicalStreamReader`”类未发现异常？

```java
 Caused by: java.lang.ClassNotFoundException: com.thoughtworks.xstream.io.HierarchicalStreamReader
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1516)
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1361)
	at java.lang.ClassLoader.loadClassInternal(ClassLoader.java:320)
	... 49 more 
```

 ## 解决办法

类“`HierarchicalStreamReader`”属于“ **xstream.jar** ”。如果您使用的是 Maven，那么在您的`pom.xml`文件中声明以下依赖关系。

```java
 <dependency>
		<groupId>com.thoughtworks.xstream</groupId>
		<artifactId>xstream</artifactId>
		<version>1.3.1</version>
	</dependency> 
```

**Note**
For Ant user, just download the “**xstream.jar**” from [http://xstream.codehaus.org/](http://web.archive.org/web/20190306164910/http://xstream.codehaus.org/) directly.[spring mvc](http://web.archive.org/web/20190306164910/http://www.mkyong.com/tag/spring-mvc/)![](img/8ef572c171c329152a7f57f2f7cbc16c.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190306164910/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="9745">







