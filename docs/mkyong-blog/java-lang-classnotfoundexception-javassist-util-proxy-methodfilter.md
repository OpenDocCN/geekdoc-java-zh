# Java . lang . classnotfoundexception:javassist . util . proxy . method filter

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/java-lang-classnotfoundexception-javassist-util-proxy-methodfilter/>

## 问题

使用 Hibernate 3.6.3，但是遇到这个 **javassist not found 错误**，错误堆栈见下文:

```java
 Caused by: java.lang.NoClassDefFoundError: javassist/util/proxy/MethodFilter
	...
	at org.hibernate.tuple.entity.PojoEntityTuplizer.<init>(PojoEntityTuplizer.java:77)
	... 16 more
Caused by: java.lang.ClassNotFoundException: javassist.util.proxy.MethodFilter
	at java.net.URLClassLoader$1.run(URLClassLoader.java:200)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:188)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:301)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:252)
	at java.lang.ClassLoader.loadClassInternal(ClassLoader.java:320)
	... 21 more 
```

 ## 解决办法

**javassist.jar** 缺失，可以从[JBoss Maven repository](http://web.archive.org/web/20190224165917/https://repository.jboss.org/nexus/content/groups/public/)获取最新。

*文件:pom.xml*

```java
 <project ...>
	<repositories>
		<repository>
			<id>JBoss repository</id>
			<url>http://repository.jboss.org/nexus/content/groups/public/</url>
		</repository>
	</repositories>

	<dependencies>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-core</artifactId>
			<version>3.6.3.Final</version>
		</dependency>

		<dependency>
			<groupId>javassist</groupId>
			<artifactId>javassist</artifactId>
			<version>3.12.1.GA</version>
		</dependency>

	</dependencies>
</project> 
```

[hibernate](http://web.archive.org/web/20190224165917/http://www.mkyong.com/tag/hibernate/)![](img/722ca65040b555bdb0b30ccf1f1e761e.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224165917/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8576">







