# 与清单文件一起工作：基础知识

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/manifestindex.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/manifestindex.html)

JAR 文件支持广泛的功能，包括电子签名、版本控制、包封装等。是什么赋予了 JAR 文件这种多功能性？答案就是 JAR 文件的*清单*。

清单是一个特殊的文件，可以包含关于打包在 JAR 文件中的文件的信息。通过调整清单包含的这些“元”信息，您可以使 JAR 文件具有各种用途。

本课程将解释清单文件的内容，并向您展示如何使用它，包括基本功能的示例：

## 理解默认清单

创建 JAR 文件时，会自动创建一个默认清单。本节描述了默认清单。

## 修改清单文件

本节向您展示了修改清单文件的基本方法。后续章节演示了您可能想要进行的具体修改。

## 设置应用程序的入口点

本节描述了如何在清单文件中使用`Main-Class`头来设置应用程序的入口点。

## 将类添加到 JAR 文件的类路径

本节描述了如何在清单文件中使用`Class-Path`头来在运行小程序或应用程序时将其他 JAR 文件中的类添加到类路径中。

## 设置包版本信息

本节描述了如何在清单文件中使用包版本头。

## 在 JAR 文件中封装包

本节描述了如何通过修改清单文件在 JAR 文件中封装包。

## 使用清单属性增强安全性

本节描述了如何使用清单属性来增加小程序或 Java Web 启动应用程序的安全性。

## 附加信息

清单格式的[规范](https://docs.oracle.com/javase/8/docs/technotes/guides/jar/jar.html#JARManifest)是在线 JDK 文档的一部分。
