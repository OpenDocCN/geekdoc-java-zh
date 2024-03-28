# 事件分发线程

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/concurrency/dispatch.html`](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/dispatch.html)

Swing 事件处理代码在一个称为事件分发线程的特殊线程上运行。大多数调用 Swing 方法的代码也在此线程上运行。这是必要的，因为大多数 Swing 对象方法都不是“线程安全”的：从多个线程调用它们会导致线程干扰或内存一致性错误。API 规范中标记为“线程安全”的一些 Swing 组件方法可以从任何线程安全地调用。所有其他 Swing 组件方法必须从事件分发线程中调用。忽略此规则的程序可能大部分时间都能正常运行，但会出现难以复现的不可预测错误。

有用的是将在事件分发线程上运行的代码视为一系列短任务。大多数任务是调用事件处理方法，例如`ActionListener.actionPerformed`。其他任务可以由应用程序代码使用`invokeLater`或`invokeAndWait`调度。事件分发线程上的任务必须快速完成；如果不这样做，未处理的事件会积压，用户界面会变得无响应。

如果您需要确定您的代码是否在事件分发线程上运行，请调用[`javax.swing.SwingUtilities.isEventDispatchThread`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingUtilities.html#isEventDispatchThread--)。
