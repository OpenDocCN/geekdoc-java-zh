# 休眠错误–需要使用 AnnotationConfiguration 实例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-error-an-annotationconfiguration-instance-is-required-to-use/>

Hibernate 注释需要“AnnotationConfiguration”而不是普通的“Configuration()”来构建会话工厂。

```java
 INFO: Configuration resource: /hibernate.cfg.xml
Initial SessionFactory creation failed.org.hibernate.MappingException: 
An AnnotationConfiguration instance is required to use <mapping class="com.mkyong.common.Stock"/>
Exception in thread "main" java.lang.ExceptionInInitializerError
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:19)
	at com.mkyong.persistence.HibernateUtil.<clinit>(HibernateUtil.java:8)
	at com.mkyong.common.App.main(App.java:11)
Caused by: org.hibernate.MappingException: An AnnotationConfiguration instance is required to use <mapping class="com.mkyong.common.Stock"/>
	at org.hibernate.cfg.Configuration.parseMappingElement(Configuration.java:1600)
	at org.hibernate.cfg.Configuration.parseSessionFactory(Configuration.java:1555)
	at org.hibernate.cfg.Configuration.doConfigure(Configuration.java:1534)
	at org.hibernate.cfg.Configuration.doConfigure(Configuration.java:1508)
	at org.hibernate.cfg.Configuration.configure(Configuration.java:1428)
	at org.hibernate.cfg.Configuration.configure(Configuration.java:1414)
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:13)
	... 2 more 
```

## 解决办法

 ## 1.下载 Hibernate 注释库

可以从 [Hibernate 官网](http://web.archive.org/web/20190310090827/https://www.hibernate.org/397.html)下载该库

或者

在 Maven 的 pom.xml 中添加依赖项

```java
 <!-- Hibernate annotation -->
	<dependency>
		<groupId>hibernate-annotations</groupId>
		<artifactId>hibernate-annotations</artifactId>
		<version>3.3.0.GA</version>
	</dependency> 
```

页（page 的缩写）为了下载 Hibernate 注释库，您可能需要包含 JBoss 存储库。

```java
 <repositories>
    <repository>
      <id>JBoss repository</id>
      <url>http://repository.jboss.com/maven2/</url>
    </repository>
  </repositories> 
```

 ## 2.使用 AnnotationConfiguration 构建会话工厂

正常的 Hibernate XML 文件映射使用配置()

```java
 return new Configuration().configure().buildSessionFactory(); 
```

对于 Hibernate 注释，您必须将其更改为“AnnotationConfiguration”

```java
 return new AnnotationConfiguration().configure().buildSessionFactory(); 
```

##### HibernateUtil.java

使用“AnnotationConfiguration”进行 Hibernate 注释应用的“HibernateUtil.java”的完整示例。

```java
 package com.mkyong.persistence;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.AnnotationConfiguration;

public class HibernateUtil {

    private static final SessionFactory sessionFactory = buildSessionFactory();

    private static SessionFactory buildSessionFactory() {
        try {
            // Create the SessionFactory from hibernate.cfg.xml
            return new AnnotationConfiguration().configure().buildSessionFactory();

        }
        catch (Throwable ex) {
            // Make sure you log the exception, as it might be swallowed
            System.err.println("Initial SessionFactory creation failed." + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }

    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }

    public static void shutdown() {
    	// Close caches and connection pools
    	getSessionFactory().close();
    }

} 
```

[hibernate](http://web.archive.org/web/20190310090827/http://www.mkyong.com/tag/hibernate/)







