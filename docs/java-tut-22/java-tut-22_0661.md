# 创建和绘制图像

> 原文：[`docs.oracle.com/javase/tutorial/2d/images/drawonimage.html`](https://docs.oracle.com/javase/tutorial/2d/images/drawonimage.html)

我们已经知道如何加载现有图像，该图像在您的系统中创建并存储，或者在任何网络位置。但是，您可能还想创建一个新图像作为像素数据缓冲区。

在这种情况下，您可以手动创建一个 `BufferedImage` 对象，使用该类的三个构造函数：

+   `new BufferedImage(width, height, type)` - 构造一个预定义图像类型的 `BufferedImage`。

+   `new BufferedImage(width, height, type, colorModel)` - 构造一个预定义图像类型的 `BufferedImage`：`TYPE_BYTE_BINARY` 或 `TYPE_BYTE_INDEXED`。

+   `new BufferedImage(colorModel, raster, premultiplied, properties)` - 使用指定的 `ColorModel` 和 `Raster` 构造一个新的 `BufferedImage`。

另一方面，我们可以使用 `Component` 类的方法。这些方法可以分析给定 `Component` 或 `GraphicsConfiguration` 的显示分辨率，并创建一个适当类型的图像。

+   `Component.createImage(width, height)`

+   `GraphicsConfiguration.createCompatibleImage(width, height)`

+   `GraphicsConfiguration.createCompatibleImage(width, height, transparency)`

GraphicsConfiguration 返回一个 BufferedImage 类型的对象，但 Component 返回一个 Image 类型的对象，如果您需要一个 BufferedImage 对象，那么您可以在代码中执行 `instanceof` 并转换为 BufferedImage。

正如在之前的课程中已经提到的，我们不仅可以在屏幕上渲染图像。图像本身可以被视为一个绘图表面。您可以使用 `BufferedImage` 类的 `createGraphics()` 方法来实现这一目的：

```java
...

BufferedImage off_Image =
  new BufferedImage(100, 50,
                    BufferedImage.TYPE_INT_ARGB);

Graphics2D g2 = off_Image.createGraphics();

```

另一个有趣的离屏图像的用途是自动*双缓冲*。这个功能允许通过将图像绘制到后备缓冲区，然后将该缓冲区复制到屏幕上，而不是直接绘制到屏幕上，从而避免动画图像的闪烁。

Java 2D 还允许访问用于离屏图像的硬件加速，这可以提供更好的渲染性能以及从这些图像复制。您可以通过使用 `Image` 类的以下方法来获得此功能的好处：

+   `getCapabilities` 方法允许您确定图像当前是否加速。

+   `setAccelerationPriority` 方法允许您设置有关图像加速重要性的提示。

+   `getAccelerationPriority` 方法获取有关加速重要性的提示。
