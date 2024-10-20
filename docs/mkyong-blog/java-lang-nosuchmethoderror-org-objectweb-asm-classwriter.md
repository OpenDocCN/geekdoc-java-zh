# Java . lang . nosuchmethod error:org . object web . ASM . class writer

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/java-lang-nosuchmethoderror-org-objectweb-asm-classwriter/>

## 问题

在 Hibernate 开发中，经常会遇到以下错误消息。

```java
 SEVERE: Context initialization failed
org.springframework.beans.factory.BeanCreationException: 
Error creating bean with name 'sessionFactory' defined in ServletContext resource 
[/WEB-INF/classes/config/database/spring/HibernateSessionFactory.xml]: 
Invocation of init method failed; nested exception is 

Caused by: org.hibernate.HibernateException: 
Unable to instantiate default tuplizer [org.hibernate.tuple.entity.PojoEntityTuplizer]
	...
	...
Caused by: java.lang.reflect.InvocationTargetException
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(Unknown Source)
	...
Caused by: java.lang.NoSuchMethodError: org.objectweb.asm.ClassWriter.<init>(I)V
	at net.sf.cglib.core.DebuggingClassWriter.<init>(DebuggingClassWriter.java:47)
	... 
```

 ## 解决办法

“**无法实例化默认的 tu plizer[org . hibernate . tuple . entity . pojoentitytuplizer**”是一个一般性的错误消息，它可能由多种原因引起。因此，您必须查看导致错误的最后一行。

```java
 Caused by: java.lang.NoSuchMethodError: org.objectweb.asm.ClassWriter 
```

主要原因是**旧的 asm.jar 库**例如“asm-1.5.3.jar”，只需**将 asm 库**升级到最新版本就可以摆脱错误信息。例如，“asm-3.1.jar”。

 ## 参考

1.  [ASM 图书馆官方网站](http://web.archive.org/web/20190305233131/http://asm.ow2.org/)

[hibernate](http://web.archive.org/web/20190305233131/http://www.mkyong.com/tag/hibernate/)







