# Java . lang . classnotfoundexception:javax . transaction . transaction manager

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/java-lang-classnotfoundexception-javax-transaction-transactionmanager/>

## 问题

在 JPA 或 Hibernate 开发中，它会显示以下错误消息:

```java
 Caused by: java.lang.ClassNotFoundException: javax.transaction.TransactionManager
at java.net.URLClassLoader$1.run(Unknown Source)
at java.security.AccessController.doPrivileged(Native Method)
at java.net.URLClassLoader.findClass(Unknown Source)
at java.lang.ClassLoader.loadClass(Unknown Source)
at sun.misc.Launcher$AppClassLoader.loadClass(Unknown Source)
at java.lang.ClassLoader.loadClass(Unknown Source)
… 23 more 
```

## 解决办法

**javax . transaction . transaction manager**是 J2EE SDK 库“ **javaee.jar** ”中的一个类，您的项目类路径中缺少这个 jar 文件。

## 1.J2EE SDK

你总是可以从 http://java.sun.com/javaee/的 T2 那里得到 javaee.jar。在你的电脑中下载并安装 SDK， **javaee.jar** 可以在“\ J2EE _ SDK _ 文件夹\lib”文件夹中找到。举个例子，

```java
 C:\Sun\SDK\lib\javaee.jar 
```

获取 **javaee.jar** 文件，并将其包含在项目类路径中。

## 2.Java.Net 知识库

或者，您可以从 java.net 专家那里获得" **javaee.jar** "

```java
 <repositories>
  	<repository>
  		<id>Java.Net</id>
  		<url>http://download.java.net/maven/2/</url>
  	</repository>
  </repositories>

  <dependencies>
    <!-- Javaee API -->
	<dependency>
    	<groupId>javax</groupId>
    	<artifactId>javaee-api</artifactId>
    	<version>6.0</version>
	</dependency>
  </dependencies> 
```

The downloaded java.net **javaee.jar** is not contains any method bodies, see this “[how to get javaee.jar from Maven](http://web.archive.org/web/20220930231922/http://www.mkyong.com/maven/how-to-download-j2ee-api-javaee-jar-from-maven/)” article for detail.<input type="hidden" id="mkyong-current-postId" value="5875">