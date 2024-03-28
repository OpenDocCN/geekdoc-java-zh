# 观察受限应用程序

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/rstep1.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/rstep1.html)

控制应用程序快速浏览课程的最后一部分展示了如何通过使用新的`-Djava.security.manager`命令行参数调用解释器来在安全管理器下运行应用程序。但是，如果要调用的应用程序位于 JAR 文件中怎么办？

解释器选项之一是`-cp`（用于类路径）选项，它允许您为应用程序类和资源指定搜索路径。因此，要在`sCount.jar` JAR 文件内执行`Count`应用程序，并将文件`C:\TestData\data`指定为其参数，您可以在包含`sCount.jar`的目录中键入以下命令：

```java
java -cp sCount.jar Count C:\TestData\data

```

要使用安全管理器执行应用程序，请添加`-Djava.security.manager`，如下所示：

```java
java -Djava.security.manager -cp sCount.jar Count C:\TestData\data

```

* * *

**重要：** 运行此命令时，您的 Java 解释器将抛出下面显示的异常：

```java
Exception in thread "main" java.security.AccessControlException:
access denied (java.io.FilePermission C:\TestData\data read)
    at java.security.AccessControlContext.checkPermission(Compiled Code)
    at java.security.AccessController.checkPermission(Compiled Code)
    at java.lang.SecurityManager.checkPermission(Compiled Code)
    at java.lang.SecurityManager.checkRead(Compiled Code)
    at java.io.FileInputStream.<init>(Compiled Code)
    at Count.main(Compiled Code)

```

在此示例中，`AccessControlException`报告说`count`应用程序没有权限读取文件`C:\TestData\data`。您的解释器引发此异常，因为除非在`policy`文件中包含的`grant`语句中明确允许，否则它不会允许任何在安全管理器下运行的应用程序读取文件或访问其他资源。

* * *
