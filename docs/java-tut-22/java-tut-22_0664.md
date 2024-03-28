# 一个基本的打印程序

> 原文：[`docs.oracle.com/javase/tutorial/2d/printing/printable.html`](https://docs.oracle.com/javase/tutorial/2d/printing/printable.html)

本节解释了如何创建一个基本的打印程序，显示打印对话框，并将文本“Hello World”打印到所选打印机。

打印任务通常由两部分组成：

+   作业控制 — 创建打印作业，将其与打印机关联，指定副本数量，并与用户打印对话框交互。

+   页面成像 — 将内容绘制到页面上，并管理跨页的内容（分页）。

首先创建打印作业。表示打印作业和大多数其他相关类的类位于[`java.awt.print`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/package-summary.html)包中。

```java
import java.awt.print.*;

PrinterJob job = PrinterJob.getPrinterJob();

```

接下来提供代码，通过实现[`Printable`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/Printable.html)接口将内容呈现到页面上。

```java
class HelloWorldPrinter
              implements Printable { ... }
...
job.setPrintable(new HelloWorldPrinter());

```

应用程序通常会显示打印对话框，以便用户可以调整各种选项，如副本数量、页面方向或目标打印机。

```java
boolean doPrint = job.printDialog();

```

此对话框会一直显示，直到用户批准或取消打印。如果`doPrint`变量为 true，则用户已经下达打印命令。如果`doPrint`变量为 false，则用户取消了打印作业。由于显示对话框是可选的，返回的值纯粹是信息性的。

如果`doPrint`变量为 true，则应用程序将通过调用`PrinterJob.print`方法请求打印作业。

```java
if (doPrint) {
    try {
        job.print();
    } catch (PrinterException e) {
        // The job did not successfully
        // complete
    }
}

```

如果发送作业到打印机时出现问题，将抛出`PrinterException`。然而，由于`PrinterJob.print`方法一旦作业发送到打印机就会返回，用户应用程序无法检测到纸张卡住或缺纸等问题。这个作业控制样板对于基本的打印使用已经足够。

`Printable`接口只有一个方法：

```java
public int print(Graphics graphics,
           PageFormat pf, int page)
           throws PrinterException;

```

[`PageFormat`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/PageFormat.html)类描述了页面方向（纵向或横向）及其大小和可成像区域，单位为 1/72 英寸。可成像区域考虑了大多数打印机的边距限制（硬件边距）。可成像区域是这些边距内的空间，在实践中通常进一步限制以留出页眉或页脚的空间。

`page`参数是将要呈现的基于零的页码。

以下代码表示完整的`Printable`实现：

```java
import java.awt.print.*;
import java.awt.*;

public class HelloWorldPrinter
    implements Printable {

  public int print(Graphics g, PageFormat pf, int page)
      throws PrinterException {

    // We have only one page, and 'page'
    // is zero-based
    if (page > 0) {
         return NO_SUCH_PAGE;
    }

    // User (0,0) is typically outside the
    // imageable area, so we must translate
    // by the X and Y values in the PageFormat
    // to avoid clipping.
    Graphics2D g2d = (Graphics2D)g;
    g2d.translate(pf.getImageableX(), pf.getImageableY());

    // Now we perform our rendering
    g.drawString("Hello world!", 100, 100);

    // tell the caller that this page is part
    // of the printed document
    return PAGE_EXISTS;
  }
}

```

此示例的完整代码在`HelloWorldPrinter.java`中。

将[`Graphics`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics.html)实例发送到打印机基本上与将其呈现到屏幕相同。在这两种情况下，您需要执行以下步骤：

+   绘制测试字符串与描述绘制到`Graphics2D`的其它操作一样简单。

+   打印机图形具有更高的分辨率，这对大多数代码来说应该是透明的。

+   `Printable.print()` 方法由打印系统调用，就像 `Component.paint()` 方法被调用来在显示器上绘制组件一样。打印系统会在页面 0、1、..等等调用 `Printable.print()` 方法，直到 `print()` 方法返回 `NO_SUCH_PAGE`。

+   `print()` 方法可能会在文档完成之前多次以相同的页面索引被调用。当用户指定了诸如多份拷贝和逐份选项等属性时，会应用此功能。

+   PageFormat 的可打印区域决定了剪切区域。可打印区域在计算分页或者如何跨打印页面展示内容时也很重要，因为页面断点是由每页能容纳多少内容决定的。

    * * *

    **注意：** 如果用户指定了不涉及特定页面索引的不同页面范围，那么对于某些页面索引，可能会跳过对 `print()` 方法的调用。

    * * *
