# 写入/保存图像

> 原文：[`docs.oracle.com/javase/tutorial/2d/images/saveimage.html`](https://docs.oracle.com/javase/tutorial/2d/images/saveimage.html)

本课程从解释如何使用`javax.imageio`包开始，将图像从外部图像格式加载到 Java 2D 使用的内部`BufferedImage`格式。然后解释如何使用`Graphics.drawImage()`来绘制该图像，可选择进行滤镜处理。

最后一阶段是将`BufferedImage`对象保存为外部图像格式。这可能是最初由`Image I/O`类从外部图像格式加载并可能使用 Java 2D API 修改的图像，也可能是由 Java 2D 创建的图像。

`Image I/O`类提供了一种简单的方法，在以下示例中以各种图像格式保存图像：

```java
static boolean ImageIO.write(RenderedImage im, 
                             String formatName,
                             File output)  throws IOException

```

* * *

**注意：** `BufferedImage`类实现了`RenderedImage`接口。

* * *

。

`formatName`参数选择要保存`BufferedImage`的图像格式。

```java
try {
    // retrieve image
    BufferedImage bi = getMyImage();
    File outputfile = new File("saved.png");
    ImageIO.write(bi, "png", outputfile);
} catch (IOException e) {
    ...
}

```

`ImageIO.write`方法调用实现 PNG 写入的代码，即“PNG 写入插件”。术语*插件*用于`Image I/O`是可扩展的，可以支持各种格式。

但以下标准图像格式插件：JPEG、PNG、GIF、BMP 和 WBMP 始终存在。

每种图像格式都有其优点和缺点：

| 格式 | 优点 | 缺点 |
| --- | --- | --- |
| GIF | 支持动画和透明像素 | 仅支持 256 种颜色和不支持半透明 |
| PNG | 比 GIF 或 JPG 更好的选择，用于高色彩无损图像，支持半透明 | 不支持动画 |
| JPG | 适用于摄影图像 | 压缩损失大，不适合文本、屏幕截图或需要完全保留原始图像的任何应用程序 |

对于大多数应用程序来说，使用这些标准插件就足够了。它们的优点是易于获取。`Image I/O`类提供了一种插入对额外格式的支持的方式，可以使用许多这样的插件。如果您想知道系统中可加载或保存的文件格式，可以使用`ImageIO`类的`getReaderFormatNames`和`getWriterFormatNames`方法。这些方法返回一个字符串数组，列出了此 JRE 支持的所有格式。

```java
String writerNames[] = ImageIO.getWriterFormatNames();

```

返回的名称数组将包括任何已安装的附加插件，这些名称中的任何一个都可以用作格式名称来选择图像写入器。以下代码示例是一个简单版本的完整图像编辑/修饰程序，使用了修订版本的`ImageDrawingApplet.java`示例程序，可以按以下方式使用：

+   图像首先通过 Image I/O 加载

+   用户从下拉列表中选择一个滤镜，然后绘制一个新的更新图像

+   用户从下拉列表中选择保存格式

+   接下来会出现文件选择器，用户选择保存图像的位置

+   修改后的图像现在可以被其他桌面应用程序查看。

这个示例的完整代码在`SaveImage.java`中表示。

在本课程中，您仅学习了`Image I/O`的基础知识，该知识提供了广泛的支持，包括直接使用`ImageWriter`插件来实现对编码过程的更精细控制。 ImageIO 可以写入多个图像、图像元数据，并确定质量与大小之间的权衡。更多信息请参见[Java Image I/O API 指南](https://docs.oracle.com/javase/8/docs/technotes/guides/imageio/spec/title.fm.html)。
