# Lesson: 编写 JavaBeans 组件

> 原文：[`docs.oracle.com/javase/tutorial/javabeans/writing/index.html`](https://docs.oracle.com/javase/tutorial/javabeans/writing/index.html)

编写 JavaBeans 组件非常容易。你不需要特殊的工具，也不必实现任何接口。编写 bean 只是遵循某些编码约定的问题。你所需要做的就是让你的类*看起来*像一个 bean —— 使用 bean 的工具将能够识别和使用你的 bean。

然而，NetBeans 提供了一些功能，使编写 bean 更容易。此外，Java SE API 包括一些支持类来帮助实现常见任务。

本课程中的代码示例基于一个简单的图形组件称为`FaceBean`。

FaceBean 源代码仅包括：

`FaceBean.java`

包括 FaceBean 源代码的整个 NetBeans 项目：

`FaceBean.zip`

一个 bean 是一个遵循 JavaBeans 指南的方法名的 Java 类。一个 bean 构建工具使用*内省*来检查 bean 类。基于这种检查，bean 构建工具可以找出 bean 的属性、方法和事件。

以下各节描述了 JavaBeans 指南中关于属性、方法和事件的内容。最后，一个关于`BeanInfo`的部分展示了如何定制开发者与你的 bean 的体验。
