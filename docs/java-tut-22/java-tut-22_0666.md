# 打印多页文档

> 原文：[`docs.oracle.com/javase/tutorial/2d/printing/set.html`](https://docs.oracle.com/javase/tutorial/2d/printing/set.html)

您已经学会了如何使用[`Printable`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/Printable.html)接口打印单页文档。但是，文档通常不止一页。*分页*是识别文档中分页位置并相应打印的过程。

如果要打印多个图形图像，每页一个，使用页面索引来遍历这些页面，并在每页上打印一个。例如，如果几个图像在以下数组中表示：

```java
BufferedImage[] images = new BufferedImage[10];

```

然后使用以下代码片段中显示的`print()`方法：

```java
public int print(Graphics graphics,
           PageFormat pageFormat, int pageIndex)
           throws PrinterException {

    if (pageIndex < images.length) {
        graphics.drawImage(images[pageIndex], 100, 100, null);
        return PAGE_EXISTS;
    } else {
        return NO_SUCH_PAGE:
    }
}

```

如果文档是连续的，应用程序必须计算每页可以容纳多少内容，并在该点分页。如果文本文档由许多行组成，则应用程序必须计算这些行中有多少可以完全适合一页。[`Point`](https://docs.oracle.com/javase/8/docs/api/java/awt/Point.html)类创建一个表示位置的点(x,y)

要计算单行文本的高度，请使用[`FontMetrics`](https://docs.oracle.com/javase/8/docs/api/java/awt/FontMetrics.html)类。

```java
Font font = new Font("Serif", Font.PLAIN, 10);
FontMetrics metrics = graphics.getFontMetrics(font);
int lineHeight = metrics.getHeight();

```

`PageFormat`参数描述了页面的可打印区域。特别是，要找到页面的垂直跨度，请使用以下代码片段：

```java
double pageHeight = pageFormat.getImageableHeight();

```

使用以下代码片段计算一页上适合的行数和分页数：

```java
int linesPerPage = ((int)pageHeight)/lineHeight);
int numBreaks = (textLines.length-1)/linesPerPage;
int[] pageBreaks = new int[numBreaks];
for (int b=0; b < numBreaks; b++) {
    pageBreaks[b] = (b+1)*linesPerPage; 
}

```

使用`print()`方法计算以下原因的可打印区域：

+   文本测量取决于`FontRenderContext`，这在打印机图形返回的`FontMetrics`对象中是隐含的，除了在`print()`方法内部不可用。

+   直到打印发生，页面格式可能不会被揭示。因为如果用户在打印对话框中选择了横向模式，则需要考虑此设置。传递给`print()`方法的`PageFormat`对象提供了此信息。

分页位置如下代码片段所示：

```java
/* Draw each line that is on this page.
 * Increment 'y' position by lineHeight
 * for each line.
 */
int y = 0; 
int start = (pageIndex == 0) ? 0 : pageBreaks[pageIndex-1];
int end   = (pageIndex == pageBreaks.length)
                 ? textLines.length : pageBreaks[pageIndex];
for (int line=start; line<end; line++) {
    y += lineHeight;
    g.drawString(textLines[line], 0, y);
}

```

如果一个文档包含 100 行，每页只能容纳 48 行，则应用程序将打印 3 页，每页在第 48 行和第 96 行之后分页。剩余的 4 行将打印在最后一页上。此示例的完整代码在`PaginationExample.java`中。

`PaginationExample`代码中使用了以下简化因素：

+   每页具有相同的高度。

+   使用相同的字体。
