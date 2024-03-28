# 使用 JAR 文件：基础知识

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/basicsindex.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/basicsindex.html)

JAR 文件使用 ZIP 文件格式打包，因此您可以将它们用于诸如无损数据压缩、存档、解压缩和存档解包等任务。这些任务是 JAR 文件的最常见用途之一，您可以仅使用这些基本功能实现许多 JAR 文件的好处。

即使您想利用 JAR 文件格式提供的高级功能，如电子签名，您也需要首先熟悉基本操作。

要执行 JAR 文件的基本任务，您需要使用作为 Java 开发工具包（JDK）的一部分提供的 Java 存档工具。因为 Java 存档工具是通过使用`jar`命令调用的，所以本教程将其称为“Jar 工具”。

作为本节将涵盖的一些主题的概要和预览，以下表格总结了常见的 JAR 文件操作：

**常见的 JAR 文件操作**

| 操作 | 命令 |
| --- | --- |
| 创建 JAR 文件 | `jar cf *jar-file input-file(s)*` |
| 查看 JAR 文件的内容 | `jar tf *jar-file*` |
| 提取 JAR 文件的内容 | `jar xf *jar-file*` |
| 从 JAR 文件中提取特定文件 | `jar xf *jar-file archived-file(s)*` |
| 运行打包为 JAR 文件的应用程序（需要`Main-class`清单头） | `java -jar *app.jar*` |
| 调用打包为 JAR 文件的小程序 |

```java
<applet code=*AppletClassName.class*
        archive="*JarFileName.jar*"
        width=*width* height=*height*>
</applet>

```

|

本节向您展示如何执行最常见的 JAR 文件操作，并为每个基本功能提供示例：

## 创建 JAR 文件

本节向您展示如何使用 Jar 工具将文件和目录打包成 JAR 文件。

## 查看 JAR 文件的内容

您可以显示 JAR 文件的目录以查看其包含的内容，而无需实际解压 JAR 文件。

## 提取 JAR 文件的内容

您可以使用 Jar 工具来解压缩 JAR 文件。在提取文件时，Jar 工具会复制所需文件并将其写入当前目录，重现文件在存档中的目录结构。

## 更新 JAR 文件

本节向您展示如何通过修改其清单或添加文件来更新现有 JAR 文件的内容。

## 运行打包为 JAR 的软件

本节向您展示如何调用和运行打包在 JAR 文件中的小程序和应用程序。

## 附加参考

JDK 的文档包括 Jar 工具的参考页面：

+   [Windows 平台的 Jar 工具参考](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jar.html)

+   [UNIX 平台的 Jar 工具参考](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jar.html)
