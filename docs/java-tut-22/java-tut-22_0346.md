# 使用与 JAR 相关的 API

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/apiindex.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/apiindex.html)

Java 平台包含几个用于处理 JAR 文件的类。其中一些 API 包括：

+   [**java.util.jar**包](https://docs.oracle.com/javase/8/docs/api/java/util/jar/package-summary.html)

+   [**java.net.JarURLConnection**类](https://docs.oracle.com/javase/8/docs/api/java/net/JarURLConnection.html)

+   [**java.net.URLClassLoader**类](https://docs.oracle.com/javase/8/docs/api/java/net/URLClassLoader.html)

为了让您了解这些新 API 所打开的可能性，本课程将引导您了解一个名为 JarRunner 的示例应用程序的内部工作原理。

## 一个示例 - JarRunner 应用程序

JarRunner 允许您通过在命令行上指定 JAR 文件的 URL 来运行打包在 JAR 文件中的应用程序。例如，如果一个名为`TargetApp`的应用程序被打包在`http://www.example.com/TargetApp.jar`的 JAR 文件中，您可以使用以下命令运行该应用程序：

```java
java JarRunner http://www.example.com/TargetApp.jar

```

为了使`JarRunner`正常工作，它必须能够执行以下任务，所有这些任务都是通过使用新的 API 完成的：

+   访问远程 JAR 文件并与其建立通信链接。

+   检查 JAR 文件的清单，查看存档中哪个类是主类。

+   加载 JAR 文件中的类。

`JarRunner`应用程序由两个类组成，`JarRunner`和`JarClassLoader`。`JarRunner`将大部分 JAR 处理任务委托给`JarClassLoader`类。`JarClassLoader`扩展了`java.net.URLClassLoader`类。在继续本课程之前，您可以查看`JarRunner`和`JarClassLoader`类的源代码：

+   `JarRunner.java`

+   `JarClassLoader.java`

本课程分为两部分：

## JarClassLoader 类

本节展示了`JarClassLoader`如何使用一些新的 API 来执行 JarRunner 应用程序所需的任务。

## JarRunner 类

本节总结了包含`JarRunner`应用程序的`JarRunner`类。
