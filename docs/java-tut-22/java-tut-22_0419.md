# 如何使用 Swing 计时器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/misc/timer.html`](https://docs.oracle.com/javase/tutorial/uiswing/misc/timer.html)

Swing 计时器（[`javax.swing.Timer`](https://docs.oracle.com/javase/8/docs/api/javax/swing/Timer.html) 的一个实例）在指定延迟后触发一个或多个动作事件。不要将 Swing 计时器与 `java.util` 包中的通用计时器设施混淆。本页仅描述 Swing 计时器。

一般来说，我们建议在 GUI 相关任务中使用 Swing 计时器而不是通用计时器，因为所有 Swing 计时器共享相同的、预先存在的计时器线程，并且 GUI 相关任务会自动在事件分发线程上执行。但是，如果您不打算从计时器中触摸 GUI，或者需要执行长时间处理，可以使用通用计时器。

您可以以两种方式使用 Swing 计时器：

+   一次性执行任务，延迟后。

    例如，工具提示管理器使用 Swing 计时器来确定何时显示工具提示以及何时隐藏它。

+   重复执行任务。

    例如，您可以执行动画或更新显示朝向目标进展的组件。

Swing 计时器非常容易使用。创建计时器时，您指定一个动作监听器，在计时器“触发”时通知它。此监听器中的 `actionPerformed` 方法应包含您需要执行的任何任务的代码。创建计时器时，还要指定计时器触发之间的毫秒数。如果希望计时器仅触发一次，可以在计时器上调用 `setRepeats(false)`。要启动计时器，请调用其 `start` 方法。要暂停它，请调用 `stop`。

请注意 Swing 计时器的任务是在事件分发线程中执行的。这意味着任务可以安全地操作组件，但也意味着任务应该快速执行。如果任务可能需要一段时间才能执行完毕，则考虑使用 `SwingWorker` 代替或与计时器一起使用。请参阅 Swing 中的并发性 了解如何使用 `SwingWorker` 类以及在多线程程序中使用 Swing 组件的信息。

让我们看一个使用计时器定期更新组件的示例。``TumbleItem`` 小程序使用计时器定期更新其显示。 （要查看此小程序运行，请转到 如何制作小程序。此小程序首先创建并启动计时器：

```java
timer = new Timer(speed, this);
timer.setInitialDelay(pause);
timer.start(); 

```

`speed` 和 `pause` 变量代表小程序参数；在另一页配置的情况下，它们分别为 100 和 1900，因此第一个计时器事件将在大约 1.9 秒后发生，并且每 0.1 秒重复一次。通过将 `this` 指定为 `Timer` 构造函数的第二个参数，`TumbleItem` 指定它是计时器事件的动作监听器。

启动计时器后，`TumbleItem`开始在后台线程中加载一系列图像。与此同时，计时器事件开始发生，导致`actionPerformed`方法执行：

```java
public void actionPerformed(ActionEvent e) {
    //If still loading, can't animate.
    if (!worker.isDone()) {
        return;
    }

    loopslot++;

    if (loopslot >= nimgs) {
        loopslot = 0;
        off += offset;

        if (off < 0) {
            off = width - maxWidth;
        } else if (off + maxWidth > width) {
            off = 0;
        }
    }

    animator.repaint();

    if (loopslot == nimgs - 1) {
        timer.restart();
    }
}

```

直到图像加载完成，`worker.isDone`返回`false`，因此计时器事件实际上被忽略了。事件处理代码的第一部分只是设置了在动画控件的`paintComponent`方法中使用的值：`loopslot`（动画中下一个图形的索引）和`off`（下一个图形的水平偏移量）。

最终，`loopslot`将达到图像数组的末尾并重新开始。当这种情况发生时，`actionPerformed`末尾的代码重新启动计时器。这样做会导致动画序列再次开始之前有一个短暂的延迟。
