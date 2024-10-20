# Java . lang . classnotfoundexception:org . spring framework . transaction . transaction exception

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring/java-lang-classnotfoundexception-org-springframework-transaction-transactionexception/>

用 Spring 3 开发 Quartz，并点击以下错误信息。

```java
 Caused by: 
       java.lang.NoClassDefFoundError: org/springframework/transaction/TransactionException
	at java.lang.Class.getDeclaredConstructors0(Native Method)
	at java.lang.Class.privateGetDeclaredConstructors(Class.java:2389)
	at java.lang.Class.getConstructor0(Class.java:2699)
	at java.lang.Class.getDeclaredConstructor(Class.java:1985)
        .....
Caused by: java.lang.ClassNotFoundException: org.springframework.transaction.TransactionException
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1711)
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1556)
	... 29 more 
```

## 解决办法

与 Quartz 无关，上面的错误消息表明您需要 Spring 事务依赖。要修复它，只需包含`spring-tx.jar`。

例如，pom.xml

```java
 <dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-tx</artifactId>
	<version>3.1.2.RELEASE</version>
   </dependency> 
```

[spring](http://web.archive.org/web/20190205233956/http://www.mkyong.com/tag/spring/)![](img/36571df22b9bbedce6b2ed920193d0a1.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190205233956/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="11231">







