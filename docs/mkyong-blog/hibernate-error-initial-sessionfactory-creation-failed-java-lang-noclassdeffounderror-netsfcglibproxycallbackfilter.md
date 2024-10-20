# 休眠错误-初始会话工厂创建失败。Java . lang . noclassdeffounderror:net/SF/cglib/proxy/CallbackFilter

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-error-initial-sessionfactory-creation-failed-java-lang-noclassdeffounderror-netsfcglibproxycallbackfilter/>

一个常见的 Hibernate 错误，这是由于缺少依赖库 cglib 造成的。

```java
 Initial SessionFactory creation failed.java.lang.NoClassDefFoundError: net/sf/cglib/proxy/CallbackFilter
Exception in thread "main" java.lang.ExceptionInInitializerError
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:18)
	at com.mkyong.persistence.HibernateUtil.<clinit>(HibernateUtil.java:8)
	at com.mkyong.common.App.main(App.java:17)
Caused by: java.lang.NoClassDefFoundError: net/sf/cglib/proxy/CallbackFilter
	at org.hibernate.bytecode.cglib.BytecodeProviderImpl.getProxyFactoryFactory(BytecodeProviderImpl.java:33)
	at org.hibernate.tuple.entity.PojoEntityTuplizer.buildProxyFactoryInternal(PojoEntityTuplizer.java:182)
	at org.hibernate.tuple.entity.PojoEntityTuplizer.buildProxyFactory(PojoEntityTuplizer.java:160)
	at org.hibernate.tuple.entity.AbstractEntityTuplizer.<init>(AbstractEntityTuplizer.java:135)
	at org.hibernate.tuple.entity.PojoEntityTuplizer.<init>(PojoEntityTuplizer.java:55)
	at org.hibernate.tuple.entity.EntityEntityModeToTuplizerMapping.<init>(EntityEntityModeToTuplizerMapping.java:56)
	at org.hibernate.tuple.entity.EntityMetamodel.<init>(EntityMetamodel.java:295)
	at org.hibernate.persister.entity.AbstractEntityPersister.<init>(AbstractEntityPersister.java:434)
	at org.hibernate.persister.entity.SingleTableEntityPersister.<init>(SingleTableEntityPersister.java:109)
	at org.hibernate.persister.PersisterFactory.createClassPersister(PersisterFactory.java:55)
	at org.hibernate.impl.SessionFactoryImpl.<init>(SessionFactoryImpl.java:226)
	at org.hibernate.cfg.Configuration.buildSessionFactory(Configuration.java:1294)
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:13)
	... 2 more
Caused by: java.lang.ClassNotFoundException: net.sf.cglib.proxy.CallbackFilter
	at java.net.URLClassLoader$1.run(URLClassLoader.java:200)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:188)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:301)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:252)
	at java.lang.ClassLoader.loadClassInternal(ClassLoader.java:320)
	... 15 more 
```

## 解决办法

你可以在这里下载图书馆-[http://cglib.sourceforge.net/](http://web.archive.org/web/20190110062218/http://cglib.sourceforge.net/)

或者

在 Maven 的 pom.xml 中添加依赖项

```java
 <dependency>
		<groupId>cglib</groupId>
		<artifactId>cglib</artifactId>
		<version>2.2</version>
	</dependency> 
```

[hibernate](http://web.archive.org/web/20190110062218/http://www.mkyong.com/tag/hibernate/)![](img/a8bac1beb519b6087cf76d20b1285381.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190110062218/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="2479">







