# java.lang.ClassFormatError:类文件中非本机或抽象的方法中缺少代码属性…

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/java-lang-classformaterror-absent-code-attribute-in-method-that-is-not-native-or-abstract-in-class-file/>

## 问题

一个非常奇怪和罕见的问题，发生在 JPA 或 Hibernate 开发中。

```java
 Caused by: java.lang.ClassFormatError: Absent Code attribute in method that is 
        not native or abstract in class file javax/persistence/GenerationType

	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClassCond(Unknown Source)
	at java.lang.ClassLoader.defineClass(Unknown Source)
	at java.security.SecureClassLoader.defineClass(Unknown Source)
	at java.net.URLClassLoader.defineClass(Unknown Source)
	at java.net.URLClassLoader.access$000(Unknown Source)
	at java.net.URLClassLoader$1.run(Unknown Source)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(Unknown Source)
	at java.lang.ClassLoader.loadClass(Unknown Source)
	at sun.misc.Launcher$AppClassLoader.loadClass(Unknown Source)
	at java.lang.ClassLoader.loadClass(Unknow 
```

 ## 解决办法

这总是由位于 Java.net 的 **javaee.jar** 引起的。许多开发人员喜欢用下面的 Maven 坐标抓取 **javaee.jar** :

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

但是，来自 java.net 的 **javaee.jar** 不包含任何方法体，只包含 API 名称。它不适合与您的应用程序一起运行或部署。

The good practice is always get the original full version of **javaee.jar** file from the [http://java.sun.com/javaee/](http://web.archive.org/web/20190517121037/http://java.sun.com/javaee/). Just download and install the J2EE SDK, and the **javaee.jar** can be found in the “\J2EE_SDK_FOLDER\lib” folder. Include it into your local Maven repository or poject classpath will get rid of the above error message. ## 参考

1.  [http://weblogs . Java . net/blog/ludo/archive/2007/01/Java _ ee _ 5 _ APIs . html](http://web.archive.org/web/20190517121037/http://weblogs.java.net/blog/ludo/archive/2007/01/java_ee_5_apis.html)
2.  [http://forums.java.net/jive/message.jspa?messageID=226931](http://web.archive.org/web/20190517121037/http://forums.java.net/jive/message.jspa?messageID=226931)
3.  [http://jersey . 576304 . N2 . nable . com/Absent-Code-attribute-in-method-that-is-not-native-TD 2632542 . html](http://web.archive.org/web/20190517121037/http://jersey.576304.n2.nabble.com/Absent-Code-attribute-in-method-that-is-not-native-td2632542.html)

[hibernate](http://web.archive.org/web/20190517121037/https://www.mkyong.com/tag/hibernate/) [jpa](http://web.archive.org/web/20190517121037/https://www.mkyong.com/tag/jpa/)







