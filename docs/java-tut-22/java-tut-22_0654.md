# 使用渲染提示显示抗锯齿文本

> 原文：[`docs.oracle.com/javase/tutorial/2d/text/renderinghints.html`](https://docs.oracle.com/javase/tutorial/2d/text/renderinghints.html)

Java 2D 文本渲染可能受*渲染提示*的影响。

请记住最重要的文本绘制方法如下：

```java
Graphics.drawString(String s, int x, int y);

```

通常，该方法会使用纯色绘制文本字符串中的每个字形，而该字形中的每个“开启”像素都会设置为该颜色。这种绘制方式产生了最高对比度的文本，但有时会出现锯齿状（锯齿状）边缘。

*文本抗锯齿*是一种用于平滑屏幕上文本边缘的技术。Java 2D API 使应用程序能够指定是否应使用此技术以及通过将文本渲染提示应用于`Graphics`来使用哪种算法。

最常见的渲染提示会将前景（文本）颜色与文本边缘的屏幕背景像素混合。要请求此提示，应用程序必须调用以下方法：

```java
graphics2D.setRenderingHint(
        RenderingHints.KEY_TEXT_ANTIALIASING,
        RenderingHints.VALUE_TEXT_ANTIALIAS_ON);

```

以下图示说明了抗锯齿功能。

![此图表示“Hello World”字符串的抗锯齿提示。](img/bf6237ded1096f96cba33265b6b44e05.png)

如果不当使用，此方法可能使文本显得过于模糊。在这种情况下，更好的提示是以下提示：

```java
graphics2D.setRenderingHint(
        RenderingHints.KEY_TEXT_ANTIALIASING,
        RenderingHints.VALUE_TEXT_ANTIALIAS_GASP);

```

这种方法会自动使用字体本身的信息来决定是使用抗锯齿还是使用纯色。

LCD 显示器具有 Java 2D API 可以利用的属性，以产生不像典型抗锯齿那样模糊的文本，但在小尺寸下更易读的文本。要求使用典型 LCD 显示器的子像素 LCD 文本模式绘制文本，应用程序必须调用以下方法：

```java
graphics2D.setRenderingHint(
        RenderingHints.KEY_TEXT_ANTIALIASING,
        RenderingHints.VALUE_TEXT_ANTIALIAS_LCD_HRGB);

```

下面所示的代码示例说明了抗锯齿功能的顺序：

1.  抗锯齿已关闭。

1.  抗锯齿已开启。

1.  使用`TEXT_ANTIALIAS_GASP`提示进行抗锯齿。

    * * *

    **注意：** 因此，GASP 表指定仅在这些大小上使用提示，而不是“平滑”。因此，在许多情况下，结果文本显示等同于`VALUE_TEXT_ANTIALIAS_OFF`。

    * * *

1.  使用`TEXT_ANTIALIAS_LCD_HRGB`提示进行抗锯齿。

<applet code="AntialiasedText" archive="examples/lib/AntialiasedTextApplet.jar" alt="AntialiasedText applet" width="300" height="180"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果看不到 applet 运行，请至少安装[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

此 applet 的完整代码在`AntialiasedText.java`中。
