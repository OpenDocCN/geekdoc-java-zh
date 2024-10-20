# 休眠错误-初始会话工厂创建失败。Java . lang . noclassdeffounderror:com/m change/v2/c3p 0/data sources

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-errorinitial-sessionfactory-creation-failed-java-lang-noclassdeffounderror-commchangev2c3p0datasources/>

Hibernate 的“C3P0”连接池错误，这是由于缺少依赖库–C3 P0 造成的。

```java
 Initial SessionFactory creation failed.java.lang.NoClassDefFoundError: com/mchange/v2/c3p0/DataSources
Exception in thread "main" java.lang.ExceptionInInitializerError
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:19)
	at com.mkyong.persistence.HibernateUtil.<clinit>(HibernateUtil.java:8)
	at com.mkyong.common.App.main(App.java:11)
Caused by: java.lang.NoClassDefFoundError: com/mchange/v2/c3p0/DataSources
	at org.hibernate.connection.C3P0ConnectionProvider.configure(C3P0ConnectionProvider.java:154)
	at org.hibernate.connection.ConnectionProviderFactory.newConnectionProvider(ConnectionProviderFactory.java:124)
	at org.hibernate.connection.ConnectionProviderFactory.newConnectionProvider(ConnectionProviderFactory.java:56)
	at org.hibernate.cfg.SettingsFactory.createConnectionProvider(SettingsFactory.java:410)
	at org.hibernate.cfg.SettingsFactory.buildSettings(SettingsFactory.java:62)
	at org.hibernate.cfg.Configuration.buildSettings(Configuration.java:2009)
	at org.hibernate.cfg.Configuration.buildSessionFactory(Configuration.java:1292)
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:13)
	... 2 more
Caused by: java.lang.ClassNotFoundException: com.mchange.v2.c3p0.DataSources
	at java.net.URLClassLoader$1.run(URLClassLoader.java:200)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:188)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:301)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:252)
	at java.lang.ClassLoader.loadClassInternal(ClassLoader.java:320)
	... 10 more 
```

## 解决办法

你可以在这里下载图书馆-[http://www.mchange.com/projects/c3p0/](http://web.archive.org/web/20190308011746/http://www.mchange.com/projects/c3p0/)

或者

使用 Maven 的坐标

```java
 <dependency>
		<groupId>c3p0</groupId>
		<artifactId>c3p0</artifactId>
		<version>0.9.1.2</version>
	</dependency> 
```

 ## 追踪

 [如何在 Hibernate 中配置 C3P0 连接池](http://web.archive.org/web/20190308011746/http://www.mkyong.com/hibernate/how-to-configure-the-c3p0-connection-pool-in-hibernate/)

[hibernate](http://web.archive.org/web/20190308011746/http://www.mkyong.com/tag/hibernate/)![](img/6bde2d55311a53938cf41bca0dc7ef16.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190308011746/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="2557">







