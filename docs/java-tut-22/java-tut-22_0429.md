# 课程：在容器内布置组件

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/layout/index.html`](https://docs.oracle.com/javase/tutorial/uiswing/layout/index.html)

示例索引

本课程告诉您如何使用 Java 平台提供的布局管理器。它还告诉您如何使用绝对定位（无布局管理器）并提供编写自定义布局管理器的示例。对于每个布局管理器（或缺乏布局管理器），本课程指向一个您可以使用 Java™ Web Start 运行的示例。通过调整示例窗口的大小，您可以看到大小变化如何影响布局。

* * *

**注意：** 本课程涵盖了手动编写布局代码，这可能具有挑战性。如果您不想学习布局管理的所有细节，您可能更喜欢使用`GroupLayout`布局管理器结合构建工具来布置您的 GUI。其中一种构建工具是 NetBeans IDE。否则，如果您想手动编码并且不想使用`GroupLayout`，那么建议使用`GridBagLayout`作为下一个最灵活和强大的布局管理器。

* * *

如果您有兴趣使用 JavaFX 创建 GUI，请参阅[JavaFX 中的布局使用](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html)。

## 布局管理器可视化指南

本节展示了标准布局管理器的示例，并指向每个布局管理器的操作指南部分。

## 使用布局管理器

本节提供了使用标准布局管理器的一般规则。它包括如何设置布局管理器，向容器添加组件，提供大小和对齐提示，放置组件之间的空间，并设置容器布局的方向，使其适合程序运行的区域设置。它还提供了一些选择正确布局管理器的提示。

## 布局管理工作原理

本节介绍了典型布局序列，然后描述了组件大小更改时会发生什么。

## 如何使用...

这一系列部分告诉您如何使用 Java 平台提供的每个通用布局管理器。

## 创建自定义布局管理器

您可以编写自己的布局管理器，而不是使用 Java 平台的布局管理器之一。

布局管理器必须实现`LayoutManager`接口，该接口规定了每个布局管理器必须定义的五个方法。可选地，布局管理器可以实现`LayoutManager2`，这是`LayoutManager`的子接口。

## 不使用布局管理器（绝对定位）

如果必要，您可以在不使用布局管理器的情况下定位组件。通常，此解决方案用于为组件指定绝对大小和位置。

## 解决常见布局问题

一些最常见的布局问题涉及显示过小或根本不显示的组件。本节告诉您如何解决这些和其他常见的布局问题。

## 问题和练习

尝试这些问题和练习，测试您在本课程中学到的知识。

如果您有兴趣使用 JavaFX 创建您的 GUI，请参阅[在 JavaFX 中使用布局](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html)。
