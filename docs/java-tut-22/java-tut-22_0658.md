# 教程：处理图像

> 原文：[`docs.oracle.com/javase/tutorial/2d/images/index.html`](https://docs.oracle.com/javase/tutorial/2d/images/index.html)

正如您已经从 图像 教程中了解的那样，`Image` 由以像素为单位的宽度和高度描述，并且具有与绘图表面无关的坐标系。

处理图像时有许多常见任务。

+   将外部 GIF、PNG JPEG 图像格式文件加载到 Java 2D 使用的内部图像表示中。

+   直接创建 Java 2D 图像并对其进行渲染。

+   将 Java 2D 图像的内容绘制到绘图表面上。

+   将 Java 2D 图像的内容保存到外部 GIF、PNG 或 JPEG 图像文件中。

本课程教授如何加载、显示和保存图像的基础知识。

您必须了解的两个主要类来处理图像：

+   [`java.awt.Image`](https://docs.oracle.com/javase/8/docs/api/java/awt/Image.html) 类是表示图形图像的像素矩形数组的超类。

+   [`java.awt.image.BufferedImage`](https://docs.oracle.com/javase/8/docs/api/java/awt/image/BufferedImage.html) 类扩展了 `Image` 类，允许应用程序直接操作图像数据（例如，检索或设置像素颜色）。应用程序可以直接构造此类的实例。

`BufferedImage` 类是 Java 2D 立即模式成像 API 的基石。它管理内存中的图像，并提供存储、解释和获取像素数据的方法。由于 `BufferedImage` 是 `Image` 的子类，因此可以通过接受 `Image` 参数的 `Graphics` 和 `Graphics2D` 方法来呈现它。

`BufferedImage` 本质上是具有可访问数据缓冲区的 `Image`。因此，直接使用 `BufferedImage` 更有效。`BufferedImage` 具有 *ColorModel* 和图像数据的 *Raster*。ColorModel 提供图像像素数据的颜色解释。

Raster 执行以下功能：

+   表示图像的矩形坐标

+   在内存中维护图像数据

+   提供从单个图像数据缓冲区创建多个子图像的机制

+   提供访问图像内特定像素的方法

图像的基本操作在以下部分中表示：

## 读取/加载图像

本节解释了如何使用 Image I/O API 将外部图像格式的图像加载到 Java 应用程序中。

## 绘制图像

本节教授如何使用 `Graphics` 和 `Graphics2D` 类的 `drawImage` 方法显示图像。

## 创建和绘制到图像

本节描述了如何创建图像以及如何将图像本身用作绘图表面。

## 写入/保存图像

本节解释了如何以适当的格式保存创建的图像。
