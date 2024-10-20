# Java . lang . unsupportedclassversionerror

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-lang-unsupportedclassversionerror/>

启动一个 Java 类，点击这个`java.lang.UnsupportedClassVersionError`，什么是类文件版本 52 56？

```java
 Error: A JNI error has occurred, please check your installation and try again

Exception in thread "main" java.lang.UnsupportedClassVersionError: 
TestHello has been compiled by a more recent version of the Java Runtime (class file version 56.0), 
this version of the Java Runtime only recognizes class file versions up to 52.0
        at java.lang.ClassLoader.defineClass1(Native Method)
        at java.lang.ClassLoader.defineClass(ClassLoader.java:763)
        at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
        at java.net.URLClassLoader.defineClass(URLClassLoader.java:468)
        at java.net.URLClassLoader.access$100(URLClassLoader.java:74)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:369)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:363)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:362)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:495) 
```

## 解决办法

Java 版本不匹配，这意味着你用`JDK 12 (56)`编译 Java 类，但试图在`JDK 8 (52)`中运行，从[维基百科](http://web.archive.org/web/20220322153341/https://en.wikipedia.org/wiki/Java_class_file)中阅读

```java
 Java SE 13 = 57 (0x39 hex)
Java SE 12 = 56 (0x38 hex)
Java SE 11 = 55 (0x37 hex)
Java SE 10 = 54 (0x36 hex)
Java SE 9 = 53 (0x35 hex)
Java SE 8 = 52 (0x34 hex)
Java SE 7 = 51 (0x33 hex)
Java SE 6.0 = 50 (0x32 hex)
Java SE 5.0 = 49 (0x31 hex)
JDK 1.4 = 48 (0x30 hex)
JDK 1.3 = 47 (0x2F hex)
JDK 1.2 = 46 (0x2E hex)
JDK 1.1 = 45 (0x2D hex) 
```

**修复**，用`--release`选项编译 Java 类，目标是 Java 8。

```java
 javac anyName.java --release 8 
```

或者 Maven。

pom.xml

```java
 <plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-compiler-plugin</artifactId>
		<version>3.8.0</version>
		<configuration>
			<source>1.8</source>
			<target>1.8</target>
		</configuration>
	</plugin> 
```

**Note**
The key is to make sure both the compile and runtime is using the same JDK.

## 参考

*   [维基百科–Java 版本历史](http://web.archive.org/web/20220322153341/https://en.wikipedia.org/wiki/Java_version_history)
*   [维基百科–Java 类文件](http://web.archive.org/web/20220322153341/https://en.wikipedia.org/wiki/Java_class_file)

<input type="hidden" id="mkyong-current-postId" value="15063">