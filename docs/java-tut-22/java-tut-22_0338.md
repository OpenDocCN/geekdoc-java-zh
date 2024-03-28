# 将类添加到 JAR 文件的类路径中

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/downman.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/downman.html)

你可能需要在一个 JAR 文件中引用其他 JAR 文件中的类。

举例来说，在一个典型的情况下，一个小程序被打包在一个 JAR 文件中，其清单引用了另一个 JAR 文件（或者几个不同的 JAR 文件），这些文件作为该小程序的工具。

你可以在小程序或应用程序的清单文件中的`Class-Path`头部字段中指定要包含的类。`Class-Path`头部的形式如下：

```java
Class-Path: *jar1-name jar2-name directory-name/jar3-name*

```

通过在清单中使用`Class-Path`头部，你可以避免在调用 Java 运行应用程序时需要指定长长的`-classpath`标志。

* * *

**注意：** `Class-Path`头部指向本地网络上的类或 JAR 文件，而不是 JAR 文件中的 JAR 文件或通过互联网协议可访问的类。要将 JAR 文件中的类加载到类路径中，你必须编写自定义代码来加载这些类。例如，如果`MyJar.jar`包含另一个名为`MyUtils.jar`的 JAR 文件，你不能使用`MyJar.jar`清单中的`Class-Path`头部来将`MyUtils.jar`中的类加载到类路径中。

* * *

## 一个例子

我们希望将`MyUtils.jar`中的类加载到类路径中，以便在`MyJar.jar`中使用。这两个 JAR 文件在同一个目录中。

我们首先创建一个名为`Manifest.txt`的文本文件，内容如下：

```java
Class-Path: MyUtils.jar

```

* * *

**警告：** 文本文件必须以一个新行或回车符结束。如果最后一行没有以新行或回车符结束，它将无法被正确解析。

* * *

然后我们通过输入以下命令创建一个名为`MyJar.jar`的 JAR 文件：

```java
jar cfm MyJar.jar Manifest.txt MyPackage/*.class

```

这将创建一个带有以下内容的清单的 JAR 文件：

```java
Manifest-Version: 1.0
Class-Path: MyUtils.jar
Created-By: 1.7.0_06 (Oracle Corporation)

```

当你运行`MyJar.jar`时，`MyUtils.jar`中的类现在已经加载到类路径中。
