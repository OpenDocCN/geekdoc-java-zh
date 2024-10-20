# ClassNotFoundException:DefaultSavedRequest

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-security/classnotfoundexception-defaultsavedrequest/>

## 问题

与 Spring Security 合作，哪个 jar 包含`DefaultSavedRequest`？

```java
 SEVERE: Exception loading sessions from persistent storage
java.lang.ClassNotFoundException: 
        org.springframework.security.web.savedrequest.DefaultSavedRequest
	at java.net.URLClassLoader$1.run(URLClassLoader.java:200)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:188)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:252) 
```

 ## 解决办法

`DefaultSavedRequest`在 **spring-security-web.jar** 里面。访问这个 [Spring Security hello world 示例](http://web.archive.org/web/20190306155926/http://www.mkyong.com/spring-security/spring-security-hello-world-example/)获得依赖库列表。

```java
 <!-- Spring Security & dependencies -->
	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-core</artifactId>
		<version>3.0.5.RELEASE</version>
	</dependency>

	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-web</artifactId>
		<version>3.0.5.RELEASE</version>
	</dependency>

	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-config</artifactId>
		<version>3.0.5.RELEASE</version>
	</dependency> 
```

[spring security](http://web.archive.org/web/20190306155926/http://www.mkyong.com/tag/spring-security/)![](img/4f73078a83b999640928d24fa1f35165.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190306155926/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="10067">







