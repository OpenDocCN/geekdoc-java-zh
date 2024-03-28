# 绘制多行文本

> 原文：[`docs.oracle.com/javase/tutorial/2d/text/drawmulstring.html`](https://docs.oracle.com/javase/tutorial/2d/text/drawmulstring.html)

如果您有一段带样式的文本，希望将其适应特定宽度，可以使用 `LineBreakMeasurer` 类。这个类使得带样式的文本可以被分成行，以便它们适应特定的视觉前进。每一行作为一个 `TextLayout` 对象返回，代表不可改变的、带样式的字符数据。然而，这个类也使得可以访问布局信息。`TextLayout` 的 `getAscent` 和 `getDescent` 方法返回有关用于在组件中定位行的字体的信息。文本被存储为 `AttributedCharacterIterator` 对象，以便字体和点大小属性可以与文本一起存储。

以下小程序使用 `LineBreakMeasurer`、`TextLayout` 和 `AttributedCharacterIterator` 在组件中定位一段带样式的文本。

<applet code="LineBreakSample" archive="examples/lib/LineBreakSampleApplet.jar" width="400" height="250" alt="LineBreakSample applet"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到小程序运行，您需要安装至少 [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

这个小程序的完整代码在 ``LineBreakSample.java`` 中。

以下代码创建一个包含字符串 `vanGogh` 的迭代器。检索迭代器的开始和结束，并从迭代器创建一个新的 `LineBreakMeasurer`。

```java
    AttributedCharacterIterator paragraph = vanGogh.getIterator();
    paragraphStart = paragraph.getBeginIndex();
    paragraphEnd = paragraph.getEndIndex();
    FontRenderContext frc = g2d.getFontRenderContext();
    lineMeasurer = new LineBreakMeasurer(paragraph, frc);

```

窗口的大小用于确定何处应该断行。同时为段落中的每一行创建一个 `TextLayout` 对象。

```java
// Set break width to width of Component.
float breakWidth = (float)getSize().width;
float drawPosY = 0;
// Set position to the index of the first
// character in the paragraph.
lineMeasurer.setPosition(paragraphStart);

// Get lines from until the entire paragraph
// has been displayed.
while (lineMeasurer.getPosition() < paragraphEnd) {

    TextLayout layout = lineMeasurer.nextLayout(breakWidth);

    // Compute pen x position. If the paragraph
    // is right-to-left we will align the
    // TextLayouts to the right edge of the panel.
    float drawPosX = layout.isLeftToRight()
        ? 0 : breakWidth - layout.getAdvance();

    // Move y-coordinate by the ascent of the
    // layout.
    drawPosY += layout.getAscent();

    // Draw the TextLayout at (drawPosX,drawPosY).
    layout.draw(g2d, drawPosX, drawPosY);

    // Move y-coordinate in preparation for next
    // layout.
    drawPosY += layout.getDescent() + layout.getLeading();
}

```

`TextLayout` 类通常不会被应用程序直接创建。然而，当应用程序需要直接处理在文本中特定位置应用了样式（文本属性）的文本时，这个类就非常有用。例如，要在段落中画一个单词斜体，应用程序需要为每个子字符串执行测量并设置字体。如果文本是双向的，这个任务就不那么容易正确完成。通过从 `AttributedString` 对象创建一个 `TextLayout` 对象来处理这个问题。请参考 Java SE 规范了解更多关于 [`TextLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/TextLayout.html) 的信息。
