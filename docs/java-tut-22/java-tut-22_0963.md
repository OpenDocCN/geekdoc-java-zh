# 全屏独占模式

> 原文：[`docs.oracle.com/javase/tutorial/extra/fullscreen/exclusivemode.html`](https://docs.oracle.com/javase/tutorial/extra/fullscreen/exclusivemode.html)

使用微软的 DirectX API 的程序员可能已经熟悉全屏独占模式。其他程序员可能对这个概念有些陌生。无论哪种情况，全屏独占模式是 J2SE™ 1.4 版本的一个强大功能，允许程序员暂停窗口系统，以便直接向屏幕绘制。

这是与许多传统 GUI 程序有些微的范式转变。在传统的 Java GUI 程序中，AWT 负责从操作系统通过*事件分发线程*传播*绘制事件*，并在适当时调用 AWT 的`Component.paint`方法。在全屏独占应用程序中，绘制通常由程序自身主动完成。此外，传统的 GUI 应用程序受限于用户选择的屏幕位深和大小。在全屏独占应用程序中，程序可以控制屏幕的位深和大小（*显示模式*）。最后，许多更高级的技术，如*翻页*（下文讨论）和立体缓冲（利用为每只眼睛使用单独一组帧的系统）在某些平台上要求应用程序首先处于全屏独占模式。

## 硬件加速图像基础知识

要理解全屏独占模式 API，您需要了解一些关于硬件加速图像的基本原理。`VolatileImage`接口封装了一个表面，该表面可能会或可能不会利用硬件加速。这些表面可能会因操作系统的要求而失去其硬件加速或内存（因此称为“易失性”）。有关易失性图像的更多信息，请参阅`VolatileImage` *教程*（即将推出）。

全屏独占模式通过`java.awt.GraphicsDevice`对象处理。要获取所有可用屏幕图形设备的列表（在单个或多监视器系统中），可以在本地`java.awt.GraphicsEnvironment`上调用`getScreenDevices`方法；对于默认（主要）屏幕（在单监视器系统中唯一的屏幕），可以调用`getDefaultScreenDevice`方法。

一旦获得了图形设备，可以调用以下方法之一：

+   `public boolean isFullScreenSupported()`

    此方法返回全屏独占模式是否可用。在不支持全屏独占模式的系统上，最好在固定大小的窗口模式下运行应用程序，而不是设置全屏窗口。

+   `public void setFullScreenWindow(Window w)`

    给定一个窗口，该方法使用该窗口进入全屏独占模式。如果全屏独占模式不可用，则窗口将定位在(0,0)并调整大小以适应屏幕。使用带有`null`参数的方法退出全屏独占模式。

## 编程提示

以下是关于使用全屏独占模式进行编程的一些建议：

+   在进入全屏独占模式之前检查`isFullScreenSupported`。如果不支持，性能可能会降低。

+   进入和退出全屏模式时，使用`try...finally`子句更加健壮。这不仅是良好的编码实践，还可以防止程序停留在全屏独占模式中的时间超过应有的时间：

    ```java
    GraphicsDevice myDevice;
    Window myWindow;

    try {
        myDevice.setFullScreenWindow(myWindow);
        ...
    } finally {
        myDevice.setFullScreenWindow(null);
    }

    ```

+   大多数全屏独占应用程序更适合使用无装饰窗口。使用`setUndecorated`方法在框架或对话框中关闭装饰。

+   全屏独占应用程序不应该是可调整大小的，因为调整全屏应用程序的大小可能会导致不可预测的（或可能危险的）行为。

+   出于安全原因，在应用程序中使用全屏独占模式时，用户必须授予`fullScreenExclusive`权限。
