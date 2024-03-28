# 显示模式

> 原文：[`docs.oracle.com/javase/tutorial/extra/fullscreen/displaymode.html`](https://docs.oracle.com/javase/tutorial/extra/fullscreen/displaymode.html)

一旦应用程序处于全屏独占模式，它可能能够利用主动设置*显示模式*。显示模式（`java.awt.DisplayMode`）由尺寸（以像素为单位的监视器宽度和高度）、位深度（每像素的位数）和刷新率（监视器更新自身的频率）组成。一些操作系统允许您同时使用多个位深度，此时特殊值`BIT_DEPTH_MULTI`用于位深度的值。此外，一些操作系统可能无法控制刷新率（或者您可能不关心刷新率设置）。在这种情况下，特殊值`REFRESH_RATE_UNKNOWN`用于刷新率值。

## 如何设置显示模式

要获取当前显示模式，只需在图形设备上调用`getDisplayMode`方法。要获取所有可能的显示模式列表，请调用`getDisplayModes`方法。无论您是否处于全屏独占模式，都可以随时调用`getDisplayMode`和`getDisplayModes`。

在尝试更改显示模式之前，您应该首先调用`isDisplayChangeSupported`方法。如果此方法返回`false`，则操作系统不支持更改显示模式。

只有在全屏独占模式下才能更改显示模式。要更改显示模式，请使用所需的显示模式调用`setDisplayMode`方法。如果显示模式不可用，不支持显示模式更改，或者您未在全屏独占模式下运行，则会抛出运行时异常。

## 更改显示模式的原因

设置显示模式的主要原因是*性能*。如果应用程序选择显示的图像与屏幕具有相同的位深度，应用程序可以运行得更快。此外，如果您可以依赖显示器具有特定尺寸，那么绘制到该显示器会简单得多，因为您不必根据用户设置的显示方式缩放或放大物体。

## 编程提示

以下是选择和设置显示模式的一些建议：

+   在尝试更改图形设备上的显示模式之前，请检查`isDisplayChangeSupported`方法返回的值。

+   在尝试更改显示模式之前，请确保您处于全屏独占模式。

+   与使用全屏模式一样，设置显示模式在`try...finally`子句中更加健壮：

    ```java
    GraphicsDevice myDevice;
    Window myWindow;
    DisplayMode newDisplayMode;
    DisplayMode oldDisplayMode 
        = myDevice.getDisplayMode();

    try {
        myDevice.setFullScreenWindow(myWindow);
        myDevice.setDisplayMode(newDisplayMode);
        ...
    } finally {
        myDevice.setDisplayMode(oldDisplayMode);
        myDevice.setFullScreenWindow(null);
    }

    ```

+   在为应用程序选择显示模式时，您可能希望保留首选显示模式列表，然后从可用显示模式列表中选择最佳显示模式。

+   作为备用方案，如果您想要的显示模式不可用，您可能希望以固定大小的窗口模式运行。
