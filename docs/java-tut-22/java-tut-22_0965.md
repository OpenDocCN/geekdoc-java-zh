# 被动渲染 vs. 主动渲染

> 原文：[`docs.oracle.com/javase/tutorial/extra/fullscreen/rendering.html`](https://docs.oracle.com/javase/tutorial/extra/fullscreen/rendering.html)

正如之前提到的，大多数全屏应用程序通常在绘制时更好地处于控制状态。在传统的窗口化 GUI 应用程序中，绘制的时间通常由操作系统处理。在窗口化环境中，这是完全合理的。窗口化应用程序不知道用户何时会移动、调整大小、暴露或被另一个窗口覆盖，直到实际发生。在 Java GUI 应用程序中，操作系统向 AWT 传递一个*绘画事件*，AWT 找出需要绘制的内容，创建一个带有适当裁剪区域的`java.awt.Graphics`对象，然后使用该`Graphics`对象调用`paint`方法：

```java
// Traditional GUI Application paint method:
// This can be called at any time, usually 
// from the event dispatch thread
public void paint(Graphics g) {
    // Use g to draw my Component
}

```

这有时被称为*被动渲染*。可以想象，这样的系统会产生很多开销，让许多性能敏感的 AWT 和 Swing 程序员感到恼火。

在全屏独占模式下，您不再需要担心窗口被调整大小、移动、暴露或遮挡（除非您忽略了我关于关闭调整大小的建议）。相反，应用程序窗口直接绘制到屏幕上（*主动渲染*）。这样做可以简化绘制过程，因为您永远不需要担心绘画事件。事实上，在全屏独占模式下，由操作系统传递的绘画事件甚至可能在不适当或不可预测的时间传递。

在全屏独占模式下，不要依赖`paint`方法，绘制代码通常更适合在*渲染循环*中完成：

```java
public void myRenderingLoop() {
    while (!done) {
        Graphics myGraphics = getPaintGraphics();
        // Draw as appropriate using myGraphics
        myGraphics.dispose();
    }
}

```

这样的渲染循环可以在任何线程中完成，可以是自己的辅助线程，也可以作为主应用程序线程的一部分。

## 编程提示

一些关于使用主动渲染的提示：

+   不要将绘制代码放在`paint`方法中。您永远不知道该方法何时会被调用！相反，使用另一个方法名，比如`render(Graphics g)`，在窗口模式下可以从`paint`方法中调用，或者在渲染循环中使用自己的图形调用。

+   使用`setIgnoreRepaint`方法在应用程序窗口和组件上关闭所有从操作系统完全分派的绘画事件，因为这些事件可能在不适当的时间调用，或者更糟糕的是，调用`paint`，这可能导致 AWT 事件线程和您的渲染循环之间的竞争条件。

+   将绘制代码与渲染循环分开，以便在全屏独占和窗口模式下都能完全运行。

+   优化您的渲染，以便您不会始终在屏幕上绘制所有内容（除非您使用翻页或双缓冲，下面将讨论）。

+   不要依赖`update`或`repaint`方法来传递绘画事件。

+   不要使用重量级组件，因为这些组件仍会产生涉及 AWT 和平台窗口系统的开销。

+   如果您使用轻量级组件，比如 Swing 组件，您可能需要稍微调整它们，以便它们使用您的`Graphics`进行绘制，而不是直接调用`paint`方法。请随意从您的渲染循环直接调用 Swing 方法，如`paintComponents`、`paintComponent`、`paintBorder`和`paintChildren`。

+   如果您只想要一个简单的全屏 Swing 或 AWT 应用程序，可以随意使用被动渲染，但请记住，在全屏独占模式下，绘制事件可能有些不可靠或不必要。此外，如果您使用被动渲染，您将无法使用更高级的技术，如翻页。最后，请务必小心避免死锁，如果您决定同时使用主动和被动渲染—这种方法并不推荐。
