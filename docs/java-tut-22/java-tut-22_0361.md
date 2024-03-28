# 教程：使用 Swing 组件

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/index.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/index.html)

示例索引

本课程为您提供了使用 Swing 组件所需的背景信息，然后描述了每个 Swing 组件。它假定您已成功编译和运行了使用 Swing 组件的程序，并且熟悉基本的 Swing 概念。这些先决条件在开始使用 Swing 和使用 NetBeans IDE 学习 Swing 中有所涵盖。

## 使用顶层容器

讨论了如何使用`JFrame`、`JDialog`和`JApplet`类共享的功能——内容窗格、菜单栏和根窗格。还讨论了*包含层次结构*，指的是顶层容器包含的组件树。

## JComponent 类

告诉你`JComponent`为其子类（几乎所有 Swing 组件）提供的功能，并提供如何利用这些功能的提示。本节以 API 表结束，描述了`JComponent`及其超类`Container`和`Component`定义的常用 API。

## 使用文本组件

描述了所有从`JTextComponent`继承的组件共享的功能和 API。如果您只是使用文本字段（格式化或非格式化）或文本区域，您可能不需要阅读本节。

## 如何...

按字母顺序列出如何使用每个 Swing 组件的部分。我们不希望您按顺序阅读这些部分。相反，我们建议在准备开始在自己的程序中使用 Swing 组件时阅读相关的“如何”部分。例如，如果您的程序需要一个框架、一个标签、一个按钮和一个颜色选择器，您应该阅读如何制作框架、如何使用标签、如何使用按钮和如何使用颜色选择器。

## 在 Swing 组件中使用 HTML

描述了如何通过使用 HTML 标签来改变 Swing 组件显示的文本的字体、颜色或其他格式。

## 使用模型

告诉您关于 Swing 模型架构。这种 Model-View-Controller（MVC）的变体意味着您可以（如果愿意）指定 Swing 组件的数据和状态如何存储和检索。其好处是能够在组件之间共享数据和状态，并大大提高显示大量数据的表格等组件的性能。

## 使用边框

边框非常方便，用于在组件的边缘周围绘制线条、标题和空白空间。（您可能已经注意到，本教程中的示例使用了许多边框。）本节告诉您如何向任何`JComponent`添加边框。

## 使用图标

许多 Swing 组件可以显示图标。通常，图标是作为`ImageIcon`类的实例实现的。

## 解决常见组件问题

本节讨论了常见组件相关问题的解决方案。

如果你对使用 JavaFX 创建 GUI 感兴趣，请参阅[使用 JavaFX 图表](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/charts.htm)和[使用 JavaFX UI 控件](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/ui_controls.htm)。
