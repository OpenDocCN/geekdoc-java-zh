# 休眠–找不到 C3P0ConnectionProvider

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-could-not-find-c3p0connectionprovider/>

## 问题

已将 Hibernate 配置为使用" [c3p0](http://web.archive.org/web/20190308011736/http://sourceforge.net/projects/c3p0/) "连接池，但遇到以下警告:

```java
 //...
2011-04-25_12:18:37.190 WARN  o.h.c.ConnectionProviderFactory - 
c3p0 properties is specificed, but could not find 

org.hibernate.connection.C3P0ConnectionProvider from the classpath,
 these properties are going to be ignored.
2011-04-25_12:18:37.191 INFO  o.h.c.DriverManagerConnectionProvider - 
Using Hibernate built-in connection pool (not for production use!)
//... 
```

貌似“`org.hibernate.connection.C3P0ConnectionProvider`”不见了？

 ## 解决办法

从 Hibernate v3.3 开始(如果没弄错的话)，“ **C3P0ConnectionProvider** ”被移到另一个 jar 文件“ **hibernate-c3p0.jar** ”。你需要包含它，以使 Hibernate 支持 c3p0 连接池。

可以从 JBoss 公共资源库下载“ **hibernate-c3p0.jar** ”。

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

		<!-- Hibernate c3p0 connection pool -->
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-c3p0</artifactId>
			<version>3.6.3.Final</version>
		</dependency>

	</dependencies>
</project> 
```

**Note**
See this detail guide – “[How to configure c3p0 connection pool in Hibernate](http://web.archive.org/web/20190308011736/http://www.mkyong.com/hibernate/how-to-configure-the-c3p0-connection-pool-in-hibernate/)“. ## 参考

1.  [http://docs . JBoss . org/hibernate/core/3.3/API/org/hibernate/connection/C3 P0 connection provider . html](http://web.archive.org/web/20190308011736/http://docs.jboss.org/hibernate/core/3.3/api/org/hibernate/connection/C3P0ConnectionProvider.html)
2.  [http://sourceforge.net/projects/c3p0/](http://web.archive.org/web/20190308011736/http://sourceforge.net/projects/c3p0/)

[hibernate](http://web.archive.org/web/20190308011736/http://www.mkyong.com/tag/hibernate/)







