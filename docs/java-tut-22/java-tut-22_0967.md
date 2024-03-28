# BufferStrategy 和 BufferCapabilities

> 原文：[`docs.oracle.com/javase/tutorial/extra/fullscreen/bufferstrategy.html`](https://docs.oracle.com/javase/tutorial/extra/fullscreen/bufferstrategy.html)

## `BufferStrategy`

在 Java 2 标准版中，您无需担心视频指针或视频内存就可以充分利用双缓冲或翻页。新的类`java.awt.image.BufferStrategy`已经添加，以便处理绘制到表面和组件的通用方式，无论使用的缓冲区数量或显示它们的技术。

缓冲策略为您提供了两种通用的绘图方法：`getDrawGraphics` 和 `show`。当您想开始绘图时，获取一个绘图对象并使用它。当您完成绘图并希望将信息呈现到屏幕上时，请调用 `show`。这两种方法被设计得相当优雅，可以很好地适应渲染循环：

```java
BufferStrategy myStrategy;

while (!done) {
    Graphics g = myStrategy.getDrawGraphics();
    render(g);
    g.dispose();
    myStrategy.show();
}

```

缓冲策略还设置了帮助您监视`VolatileImage`问题。在全屏独占模式下，`VolatileImage`问题尤为重要，因为窗口系统有时会收回它给您的视频内存。一个重要的例子是当用户在 Windows 中按下`ALT+TAB`组合键时，突然您的全屏程序在后台运行，您的视频内存就会丢失。您可以调用`contentsLost`方法来查看是否发生了这种情况。同样，当窗口系统将内存还给您时，您可以使用`contentsRestored`方法来查找。

## `BufferCapabilities`

如前所述，不同的操作系统，甚至同一操作系统上的不同显卡，都有不同的可用技术。这些*能力*被暴露出来，以便您可以为应用程序选择最佳技术。

类`java.awt.BufferCapabilities`封装了这些功能。每个缓冲策略都由其缓冲能力控制，因此为您的应用程序选择正确的缓冲能力非常关键。要找出哪些功能可用，请从您的图形设备上可用的`GraphicsConfiguration`对象调用`getBufferCapabilities`方法。

Java 2 标准版 1.4 版中提供的功能有：

+   `isPageFlipping`

    此功能返回此图形配置上是否可用硬件翻页。

+   `isFullScreenRequired`

    此功能返回在尝试硬件翻页之前是否需要全屏独占模式。

+   `isMultiBufferAvailable`

    此功能返回硬件中是否可用多缓冲（两个或更多后备缓冲区加上主表面）。

+   `getFlipContents`

    此功能返回用于执行硬件翻页的技术提示。这很重要，因为在使用的技术不同的情况下，`show`后的后备缓冲区内容也会有所不同。返回的值可以为 null（如果`isPageFlipping`返回`false`）或以下值之一。只要`isPageFlipping`方法返回 true，就可以为缓冲策略指定任何值，尽管性能会根据可用功能而变化。

+   `FlipContents.COPIED`

    此值意味着后备缓冲区的内容已复制到主表面。"翻页"可能是作为硬件位块传输执行的，这意味着硬件双缓冲可能是通过位块传输而不是真正的翻页完成的。这在理论上应该更快，或者至少与从`VolatileImage`到主表面的位块传输一样快，尽管实际效果可能有所不同。翻页后，后备缓冲区的内容与主表面相同。

+   `FlipContents.BACKGROUND`

    此值意味着后备缓冲区的内容已用背景颜色清除。可能已执行真正的翻页或位块传输。

+   `FlipContents.PRIOR`

    此值意味着后备缓冲区的内容现在是旧主表面的内容，反之亦然。通常，此值表示真正的翻页发生，尽管这并不是保证，再次强调，此操作的效果可能有所不同。

+   `FlipContents.UNKNOWN`

    此值意味着翻页后的后备缓冲区内容未定义。您可能需要尝试找出哪种技术最适合您（或者您可能不在乎），并且您肯定需要每次绘制时自己设置后备缓冲区的内容。

要为组件创建缓冲策略，请调用`createBufferStrategy`方法，提供所需的缓冲区数量（此数字包括主表面）。如果需要任何特定的缓冲技术，请提供适当的`BufferCapabilities`对象。请注意，当您使用此方法的版本时，必须在您的选择不可用时捕获`AWTException`。还要注意，这些方法仅适用于`Canvas`和`Window`。

一旦为组件创建了特定的缓冲策略，您可以使用`getBufferStrategy`方法对其进行操作。请注意，此方法也仅适用于画布和窗口。

## 编程提示

关于使用缓冲能力和缓冲策略的一些建议：

+   在`try...finally`子句中更加稳健地获取、使用和释放图形对象：

    ```java
    BufferStrategy myStrategy;

    while (!done) {
        Graphics g;
        try {
            g = myStrategy.getDrawGraphics();
            render(g);
        } finally {
            g.dispose();
        }
        myStrategy.show();
    }

    ```

+   在使用缓冲策略之前，请检查可用的功能。

+   为了获得最佳效果，请在全屏独占窗口上创建您的缓冲策略。在使用翻页之前，请确保检查`isFullScreenRequired`和`isPageFlipping`功能。

+   不要对性能做任何假设。根据需要调整你的代码，但要记住不同的操作系统和显卡具有不同的功能。对你的应用程序进行性能分析！

+   你可能想要对你的组件进行子类化，以重写`createBufferStrategy`方法。使用一个适合你的应用程序的策略选择算法。`FlipBufferStrategy`和`BltBufferStrategy`内部类是受保护的，可以被子类化。

+   不要忘记你可能会丢失你的绘图表面！在绘制之前一定要检查`contentsLost`和`contentsRestored`。所有已丢失的缓冲区在恢复时都必须重新绘制。

+   如果在 Swing 应用程序中使用缓冲策略进行双缓冲，你可能希望关闭 Swing 组件的双缓冲，因为它们已经是双缓冲的。视频内存有一定的价值，应仅在绝对必要时使用。

+   使用多个后备缓冲区可能会导致浪费。当绘制时间超过执行`show`的时间时，多缓冲仅有用。对你的应用程序进行性能分析！
