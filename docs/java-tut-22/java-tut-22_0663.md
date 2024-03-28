# 课程：打印

> 原文：[`docs.oracle.com/javase/tutorial/2d/printing/index.html`](https://docs.oracle.com/javase/tutorial/2d/printing/index.html)

由于 Java 2D API 使您能够在任何表面上绘制，因此其自然扩展是能够打印 Java 2D 图形。打印机可以被视为与显示器一样的图形设备。

Java 2D 打印 API 不仅限于打印图形。它还使您能够打印应用程序用户界面的内容。内容可以通过将原始数据发送到打印机并在 Java 2D 打印 API 的格式控制下进行打印，或者使用 Java 2D 图形 API 来打印。

在这节课中，您将探索 Java 2D 打印 API 的打印机和作业控制功能，这些功能是对渲染元素的补充。您将学习如何查找系统或网络上配置的打印机，并发现有关这些打印机的信息，例如支持的纸张尺寸，并选择这些属性进行打印和用户对话框。

参与打印的主要类和接口在[`java.awt.print`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/package-frame.html)和[`javax.print`](https://docs.oracle.com/javase/8/docs/api/javax/print/package-frame.html)包中（最后一个包允许您访问打印服务）。

基本的打印操作在以下部分中表示：

+   一个基本的打印程序 – 本节描述了`Printable`接口并呈现了一个基本的打印程序。

+   使用打印设置对话框– 本节解释了如何显示打印设置对话框。

+   打印多页文档 – 本节解释了如何使用分页打印多页文档。

+   使用打印服务和属性 – 本节教您有关打印服务的知识，如何指定打印数据格式，以及如何使用`javax.print`包创建打印作业。

+   打印用户界面的内容 – 本节解释了如何打印窗口或框架的内容。

+   Swing 组件中的打印支持 – 本节简要描述了`Swing`中相关打印功能，并引用了特定的`Swing`类和接口。
