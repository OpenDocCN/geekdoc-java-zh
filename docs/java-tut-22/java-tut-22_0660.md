# 绘制图像

> 原文：[`docs.oracle.com/javase/tutorial/2d/images/drawimage.html`](https://docs.oracle.com/javase/tutorial/2d/images/drawimage.html)

正如您已经了解的那样，`Graphics.drawImage`方法在特定位置绘制图像：

```java
boolean Graphics.drawImage(Image img,
                   int x, int y,
                   ImageObserver observer);

```

`x,y`位置指定了图像左上角的位置。`observer`参数通知应用程序异步加载的图像更新。`observer`参数通常不直接使用，对于[`BufferedImage`](https://docs.oracle.com/javase/8/docs/api/java/awt/image/BufferedImage.html)类来说，通常为 null。

描述的方法仅适用于整个图像要绘制的情况，将图像像素映射到用户空间坐标 1:1。有时应用程序需要绘制图像的一部分（子图像），或者缩放图像以覆盖绘图表面的特定区域，或在绘制之前对图像进行变换或过滤。

`drawImage()`方法的重载执行这些操作。例如，`drawImage()`方法的以下重载使您可以绘制指定图像的指定区域的尽可能多的部分，将其缩放以适合目标可绘制表面的指定区域：

```java
boolean Graphics.drawImage(Image img,
       int dstx1, int dsty1, int dstx2, int dsty2,
       int srcx1, int srcy1, int srcx2, int srcy2,
       ImageObserver observer);

```

`src`参数表示要复制和绘制的图像区域。`dst`参数显示要由源区域覆盖的目标区域。`dstx1, dsty1`坐标定义了绘制图像的位置。目标区域的宽度和高度维度由以下表达式计算：`(dstx2-dstx1), (dsty2-dsty1)`。如果源区域和目标区域的尺寸不同，Java 2D API 将根据需要进行放大或缩小。

以下代码示例将图像分成四个象限，并随机将源图像的每个象限绘制到目标的不同象限。

<applet code="JumbledImageApplet" archive="examples/lib/JumbledImageApplet.jar" width="400" height="250" alt="将图像分成四个象限，并随机将源图像的每个象限绘制到目标的不同象限。"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到小程序运行，您需要安装至少[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

此小程序的完整代码在`JumbledImageApplet.java`中。

此示例使用以下代码绘制混乱的`duke_skateboard.jpg`图像。它迭代源图像的四个子图像，依次将每个子图像绘制到随机选择的目标象限中。

```java
/* divide the image 'bi' into four rectangular
 * areas and draw each of these areas in to a
 * different part of the image, so as to jumble
 * up the image.  'cells' is an array which has
 * been populated with values which redirect
 * drawing of one subarea to another subarea.
 */
int cellWidth = bi.getWidth(null)/2;
int cellHeight = bi.getHeight(null)/2;
for (int x=0; x<2; x++) {
    int sx = x*cellWidth;
    for (int y=0; y<2; y++) {
        int sy = y*cellHeight;
        int cell = cells[x*2+y];
        int dx = (cell / 2) * cellWidth;
        int dy = (cell % 2) * cellHeight;
        g.drawImage(bi,
                    dx, dy, 
                    x+cellWidth, dy+cellHeight,
                    sx, sy,
                    sx+cellWidth, sy+cellHeight,
                    null);
    }
}

```

## 图像过滤

除了复制和缩放图像外，Java 2D API 还可以对图像进行滤镜处理。*滤镜*是通过将算法应用于源图像的像素来绘制或生成新图像。可以使用以下方法应用图像滤镜：

```java
void Graphics2D.drawImage(BufferedImage img,
                          BufferedImageOp op,
                          int x, int y)

```

`BufferedImageOp` 参数实现了滤镜。以下小程序代表了一个在文本上方绘制的图像。拖动滑块以通过图像显示更多或更少的文本，并使图像更加透明。

<applet code="SeeThroughImageApplet" archive="examples/lib/SeeThroughImageApplet.jar" width="400" height="250" alt="代表在文本上方绘制的图像"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到小程序运行，请至少安装[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

以下代码显示了如何通过使用`RescaleOp`对象对带有*alpha*通道的`BufferedImage`对象执行滤镜操作，并通过该对象重新调整 alpha 通道。alpha 通道确定每个像素的透明度。它还指定了此图像覆盖的程度。

```java
/* Create an ARGB BufferedImage */
BufferedImage img = ImageIO.read(imageSrc);
int w = img.getWidth(null);
int h = img.getHeight(null);
BufferedImage bi = new
    BufferedImage(w, h, BufferedImage.TYPE_INT_ARGB);
Graphics g = bi.getGraphics();
g.drawImage(img, 0, 0, null);

/*
 * Create a rescale filter op that makes the image
 * 50% opaque.
 */
float[] scales = { 1f, 1f, 1f, 0.5f };
float[] offsets = new float[4];
RescaleOp rop = new RescaleOp(scales, offsets, null);

/* Draw the image, applying the filter */
g2d.drawImage(bi, rop, 0, 0);

```

完整示例在`SeeThroughImageApplet.java`中包含了使用滑块调整透明度从初始 50%的代码。此示例还需要 duke_skateboard.jpg 图像。

`RescaleOp` 对象只是可以创建的许多滤镜之一。Java 2D API 具有几种内置滤镜，包括以下内容：

+   `ConvolveOp`。每个输出像素都是从源图像中周围像素计算出来的。可用于模糊或锐化图像。

+   `AffineTransformOp`。此滤镜通过在像素位置上应用变换将源中的像素映射到目标中的不同位置。

+   `LookupOp`。此滤镜使用应用提供的查找表重新映射像素颜色。

+   `RescaleOp`。此滤镜将颜色乘以某个因子。可用于使图像变亮或变暗，增加或减少其不透明度等。

以下示例使用了描述的每个滤镜以及缩放：

<applet code="ImageDrawingApplet" archive="examples/lib/ImageDrawingApplet.jar" width="400" height="250" alt="演示滤镜和缩放"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到小程序运行，请至少安装[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

此小程序的完整代码在`ImageDrawingApplet.java`中，此小程序需要 bld.jpg 图像。

使用下拉菜单选择图像缩放或滤镜操作。
