# 打印

> 原文：[`docs.oracle.com/javase/tutorial/2d/overview/printing.html`](https://docs.oracle.com/javase/tutorial/2d/overview/printing.html)

所有的 Swing 和 Java 2D 图形，包括合成图形和图像，都可以通过使用 Java 2D Printing API 渲染到打印机上。该 API 还提供文档组合功能，使您能够执行诸如更改打印页面顺序之类的操作。

渲染到打印机就像渲染到屏幕一样。打印系统控制何时渲染页面，就像绘图系统控制组件何时在屏幕上绘制一样。

Java 2D Printing API 基于*回调*模型，其中打印系统而不是应用程序控制打印页面的时间。应用程序向打印系统提供要打印的文档信息，打印系统确定何时需要成像每一页。

以下两个功能对支持打印很重要：

+   **作业控制** – 启动和管理打印作业，包括显示标准打印和设置对话框

+   **分页** – 在打印系统请求时呈现每一页

当需要成像页面时，打印系统使用适当的`Graphics`上下文调用应用程序的`print`方法。要在打印时使用 Java 2D API 功能，您将`Graphics`对象转换为`Graphics2D`类，就像在渲染到屏幕时一样。
