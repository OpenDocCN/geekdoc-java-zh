# 不兼容 class 错误:JobDetailBean 将接口 org.quartz.JobDetail 作为超类

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring/incompatibleclasschangeerror-jobdetailbean-has-interface-org-quartz-jobdetail-as-super-class/>

开发**石英 2.1.5** + **弹簧 3.1.2 .释放**，出现以下错误信息:

```java
 Caused by: java.lang.IncompatibleClassChangeError: 
	class org.springframework.scheduling.quartz.JobDetailBean has interface org.quartz.JobDetail as super class
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClassCond(ClassLoader.java:631)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:615)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:141)
	at org.apache.catalina.loader.WebappClassLoader.findClassInternal(WebappClassLoader.java:2901)
	at org.apache.catalina.loader.WebappClassLoader.findClass(WebappClassLoader.java:1170)
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1678)
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1556)
	at org.springframework.util.ClassUtils.forName(ClassUtils.java:258)
	... 19 more 
```

## 解决办法

Quartz 2 APIs 变化很大，已经有人在 Spring JIRA 上填了一个 [bug 报告](http://web.archive.org/web/20190205235332/https://jira.springsource.org/browse/SPR-8581)。这时，“**弹簧 3 与石英 2** 不兼容”。

这里有三种选择:

1.  使用石英 1.8.5，弹簧 3 与石英 1.x 大集成，经典且稳定。
2.  不使用 Spring 的`QuartzJobBean`进行集成，直接使用 Quartz 的接口/类。
3.  最后，还有什么？请等待错误修复。

[quartz](http://web.archive.org/web/20190205235332/http://www.mkyong.com/tag/quartz/) [spring](http://web.archive.org/web/20190205235332/http://www.mkyong.com/tag/spring/)![](img/6e3c34b4d849cffd37dfe7bb9d9c4a5a.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190205235332/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="11236">







