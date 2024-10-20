# 休眠错误-初始会话工厂创建失败。Java . lang . noclassdeffounderror:org/Apache/commons/collections/sequenced hashmap

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-error-initial-sessionfactory-creation-failed-java-lang-noclassdeffounderror-orgapachecommonscollectionssequencedhashmap/>

一个常见的 Hibernate 错误，这是由于缺少依赖库——Apache Common Collection 造成的。

```java
 Initial SessionFactory creation failed.java.lang.NoClassDefFoundError: org/apache/commons/collections/SequencedHashMap
Exception in thread "main" java.lang.ExceptionInInitializerError
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:18)
	at com.mkyong.persistence.HibernateUtil.<clinit>(HibernateUtil.java:8)
	at com.mkyong.common.App.main(App.java:17)
Caused by: java.lang.NoClassDefFoundError: org/apache/commons/collections/SequencedHashMap
	at org.hibernate.mapping.Table.<init>(Table.java:33)
	at org.hibernate.cfg.Mappings.addTable(Mappings.java:165)
	at org.hibernate.cfg.HbmBinder.bindRootPersistentClassCommonValues(HbmBinder.java:290)
	at org.hibernate.cfg.HbmBinder.bindRootClass(HbmBinder.java:273)
	at org.hibernate.cfg.HbmBinder.bindRoot(HbmBinder.java:144)
	at org.hibernate.cfg.Configuration.add(Configuration.java:669)
	at org.hibernate.cfg.Configuration.addInputStream(Configuration.java:504)
	at org.hibernate.cfg.Configuration.addResource(Configuration.java:566)
	at org.hibernate.cfg.Configuration.parseMappingElement(Configuration.java:1587)
	at org.hibernate.cfg.Configuration.parseSessionFactory(Configuration.java:1555)
	at org.hibernate.cfg.Configuration.doConfigure(Configuration.java:1534)
	at org.hibernate.cfg.Configuration.doConfigure(Configuration.java:1508)
	at org.hibernate.cfg.Configuration.configure(Configuration.java:1428)
	at org.hibernate.cfg.Configuration.configure(Configuration.java:1414)
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:13)
	... 2 more
Caused by: java.lang.ClassNotFoundException: org.apache.commons.collections.SequencedHashMap
	at java.net.URLClassLoader$1.run(URLClassLoader.java:200)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:188)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:301)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:252)
	at java.lang.ClassLoader.loadClassInternal(ClassLoader.java:320)
	... 17 more 
```

## 解决办法

你可以在这里下载图书馆-[http://commons.apache.org/collections/](http://web.archive.org/web/20190302163458/http://commons.apache.org/collections/)

或者

在 Maven 的 pom.xml 中添加依赖项

```java
 <dependency>
		<groupId>commons-collections</groupId>
		<artifactId>commons-collections</artifactId>
		<version>3.2.1</version>
      </dependency> 
```

[hibernate](http://web.archive.org/web/20190302163458/http://www.mkyong.com/tag/hibernate/)![](img/701ea3f40fcec428ff87586be34edabd.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190302163458/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="2473">







