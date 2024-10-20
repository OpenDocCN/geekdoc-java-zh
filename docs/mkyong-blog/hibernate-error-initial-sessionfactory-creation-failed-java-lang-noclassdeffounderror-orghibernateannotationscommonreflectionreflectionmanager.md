# Hibernate 错误-初始会话工厂创建失败。Java . lang . noclassdeffounderror:org/hibernate/annotations/common/reflection/reflection manager

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-error-initial-sessionfactory-creation-failed-java-lang-noclassdeffounderror-orghibernateannotationscommonreflectionreflectionmanager/>

这是由于缺少 Hibernate commons 注释库造成的。

```java
 Initial SessionFactory creation failed.java.lang.NoClassDefFoundError: 
org/hibernate/annotations/common/reflection/ReflectionManager
Exception in thread "main" java.lang.ExceptionInInitializerError
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:19)
	at com.mkyong.persistence.HibernateUtil.<clinit>(HibernateUtil.java:8)
	at com.mkyong.common.App.main(App.java:11)
Caused by: java.lang.NoClassDefFoundError: org/hibernate/annotations/common/reflection/ReflectionManager
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:13)
	... 2 more
Caused by: java.lang.ClassNotFoundException: org.hibernate.annotations.common.reflection.ReflectionManager
	at java.net.URLClassLoader$1.run(URLClassLoader.java:200)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:188)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:301)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:252)
	at java.lang.ClassLoader.loadClassInternal(ClassLoader.java:320)
	... 3 more 
```

## 解决办法

可以从 [Hibernate 官网](http://web.archive.org/web/20190203090236/https://www.hibernate.org/397.html)下载该库

或者

在 Maven 的 pom.xml 中添加依赖项

```java
 <dependency>
		<groupId>hibernate-commons-annotations</groupId>
		<artifactId>hibernate-commons-annotations</artifactId>
		<version>3.0.0.GA</version>
	</dependency> 
```

页（page 的缩写）为了下载 Hibernate 公共注释库，您可能需要包含 JBoss 存储库。

```java
 <repositories>
    <repository>
      <id>JBoss repository</id>
      <url>http://repository.jboss.com/maven2/</url>
    </repository>
  </repositories> 
```

[hibernate](http://web.archive.org/web/20190203090236/http://www.mkyong.com/tag/hibernate/)![](img/a44e0c4cd1af68763851a86acaf70cd0.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190203090236/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="2608">







