# 安全管理器

> 原文：[`docs.oracle.com/javase/tutorial/essential/environment/security.html`](https://docs.oracle.com/javase/tutorial/essential/environment/security.html)

*安全管理器*是为应用程序定义安全策略的对象。该策略指定了不安全或敏感的操作。安全策略不允许的任何操作都会导致抛出[`SecurityException`](https://docs.oracle.com/javase/8/docs/api/java/lang/SecurityException.html)。应用程序还可以查询其安全管理器以发现哪些操作是允许的。

通常，Web 小程序在浏览器或 Java Web Start 插件提供的安全管理器下运行。其他类型的应用程序通常在没有安全管理器的情况下运行，除非应用程序本身定义了一个。如果没有安全管理器存在，应用程序就没有安全策略，并且可以无限制地运行。

本节解释了应用程序如何与现有安全管理器交互。有关更详细的信息，包括如何设计安全管理器的信息，请参考[安全指南](https://docs.oracle.com/javase/8/docs/technotes/guides/security/index.html)。

## 与安全管理器交互

安全管理器是[`SecurityManager`](https://docs.oracle.com/javase/8/docs/api/java/lang/SecurityManager.html)类型的对象；要获取对此对象的引用，请调用`System.getSecurityManager`。

```java
SecurityManager appsm = System.getSecurityManager();

```

如果没有安全管理器，则此方法返回`null`。

一旦应用程序有了安全管理器对象的引用，它可以请求执行特定操作的权限。标准库中的许多类都会这样做。例如，`System.exit`用于终止具有退出状态的 Java 虚拟机，会调用`SecurityManager.checkExit`来确保当前线程有权限关闭应用程序。

SecurityManager 类定义了许多其他方法，用于验证其他类型的操作。例如，`SecurityManager.checkAccess`验证线程访问，`SecurityManager.checkPropertyAccess`验证对指定属性的访问。每个操作或操作组都有自己的`check*XXX*()`方法。

此外，`check*XXX*()`方法集表示已受安全管理器保护的操作集。通常，应用程序不必直接调用任何`check*XXX*()`方法。

## 识别安全违规

许多在没有安全管理器的情况下是例行操作的操作，在有安全管理器的情况下可能会抛出`SecurityException`。即使调用一个没有记录为抛出`SecurityException`的方法也是如此。例如，考虑以下用于写入文件的代码：

```java
reader = new FileWriter("xanadu.txt");

```

在没有安全管理器的情况下，此语句执行时不会出错，前提是`xanadu.txt`存在且可写。但假设此语句插入到一个通常在不允许文件输出的安全管理器下运行的 Web 小程序中。可能会导致以下错误消息：

```java
*appletviewer fileApplet.html*
    Exception in thread "AWT-EventQueue-1" java.security.AccessControlException: access denied (java.io.FilePermission xanadu.txt write)
        at java.security.AccessControlContext.checkPermission(AccessControlContext.java:323)
        at java.security.AccessController.checkPermission(AccessController.java:546)
        at java.lang.SecurityManager.checkPermission(SecurityManager.java:532)
        at java.lang.SecurityManager.checkWrite(SecurityManager.java:962)
        at java.io.FileOutputStream.<init>(FileOutputStream.java:169)
        at java.io.FileOutputStream.<init>(FileOutputStream.java:70)
        at java.io.FileWriter.<init>(FileWriter.java:46)
*...*

```

请注意，在这种情况下抛出的具体异常[`java.security.AccessControlException`](https://docs.oracle.com/javase/8/docs/api/java/security/AccessControlException.html)是`SecurityException`的子类。
