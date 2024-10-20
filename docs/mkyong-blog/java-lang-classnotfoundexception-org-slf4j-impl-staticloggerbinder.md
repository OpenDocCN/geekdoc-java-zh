# Java . lang . classnotfoundexception:org . slf4j . impl . staticloggerbinder

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/java-lang-classnotfoundexception-org-slf4j-impl-staticloggerbinder/>

## 问题

当启动一个 Wicket web 应用程序时，它显示以下错误消息:

```java
 java.lang.NoClassDefFoundError: org/slf4j/impl/StaticLoggerBinder
	at org.slf4j.LoggerFactory.getSingleton(LoggerFactory.java:223)
	at org.slf4j.LoggerFactory.bind(LoggerFactory.java:120)
	at org.slf4j.LoggerFactory.performInitialization(LoggerFactory.java:111)
	at org.slf4j.LoggerFactory.getILoggerFactory(LoggerFactory.java:269)
	at org.slf4j.LoggerFactory.getLogger(LoggerFactory.java:242)
	...
Caused by: java.lang.ClassNotFoundException: org.slf4j.impl.StaticLoggerBinder
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1516)
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1361)
	at java.lang.ClassLoader.loadClassInternal(ClassLoader.java:320)
	... 31 more 
```

## 解决办法

在 Wicket 开发中，你必须添加 **SLF4j 日志实现**，否则将无法启动。为了解决这个问题，在 Maven `pom.xml`文件中声明了 slf4j。

如果使用 log4j，那么声明 slf4j log4j 绑定:

```java
 <dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-log4j12</artifactId>
	<version>1.5.6</version>
</dependency> 
```

**For non-Maven users**
Just download the library and put it into your project classpath.

## 参考

1.  [SLF4j 参考](http://web.archive.org/web/20220805090115/http://www.slf4j.org/)

<input type="hidden" id="mkyong-current-postId" value="8926">