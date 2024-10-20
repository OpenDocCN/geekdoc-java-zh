# hibernate–不推荐使用类型注释配置

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-the-type-annotationconfiguration-is-deprecated/>

## 问题

使用 Hibernate 3.6 时，注意到前面的“`org.hibernate.cfg.AnnotationConfiguration`”被标记为“**，已弃用**”。

*代码片段……*

```java
 import org.hibernate.cfg.AnnotationConfiguration;
//...
private static SessionFactory buildSessionFactory() {
	try {

		return new AnnotationConfiguration().configure().buildSessionFactory();

	} catch (Throwable ex) {

		System.err.println("Initial SessionFactory creation failed." + ex);
		throw new ExceptionInInitializerError(ex);
	}
} 
```

代码还在工作，只是一直显示不推荐使用的警告信息，`AnnotationConfiguration`有没有替换？

 ## 解决办法

在 Hibernate 3.6 中，“`org.hibernate.cfg.AnnotationConfiguration`”已被弃用，其所有功能已被移至“`org.hibernate.cfg.Configuration`”。

因此，您可以安全地用“**配置**类替换您的“**注释配置**”。

*代码片段……*

```java
 import org.hibernate.cfg.Configuration;
//...
private static SessionFactory buildSessionFactory() {
	try {

		return new Configuration().configure().buildSessionFactory();

	} catch (Throwable ex) {

		System.err.println("Initial SessionFactory creation failed." + ex);
		throw new ExceptionInInitializerError(ex);
	}
} 
```

 ## 参考

1.  [http://docs . JBoss . org/hibernate/core/3.6/javadocs/org/hibernate/CFG/annotation configuration . html](http://web.archive.org/web/20190223085746/http://docs.jboss.org/hibernate/core/3.6/javadocs/org/hibernate/cfg/AnnotationConfiguration.html)
2.  [http://docs . JBoss . org/hibernate/core/3.6/javadocs/org/hibernate/CFG/configuration . html](http://web.archive.org/web/20190223085746/http://docs.jboss.org/hibernate/core/3.6/javadocs/org/hibernate/cfg/Configuration.html)

[deprecated](http://web.archive.org/web/20190223085746/http://www.mkyong.com/tag/deprecated/) [hibernate](http://web.archive.org/web/20190223085746/http://www.mkyong.com/tag/hibernate/)







