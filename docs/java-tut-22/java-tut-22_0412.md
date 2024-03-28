# 绑定属性和状态方法

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/concurrency/bound.html`](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/bound.html)

`SwingWorker`支持绑定属性，这对与其他线程通信非常有用。两个绑定属性已预定义：`progress`和`state`。与所有绑定属性一样，`progress`和`state`可用于在事件分发线程上触发事件处理任务。

通过实现属性更改监听器，程序可以跟踪`progress`、`state`和其他绑定属性的更改。有关更多信息，请参阅如何编写属性更改监听器中的编写事件监听器。

## `progress`绑定变量

`progress`绑定变量是一个`int`值，范围从 0 到 100。它有一个预定义的设置方法（受保护的[`SwingWorker.setProgress`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#setProgress--)）和一个预定义的获取方法（公共[`SwingWorker.getProgress`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#getProgress-int-)）。

``ProgressBarDemo``示例使用`progress`从后台任务更新`ProgressBar`控件。有关此示例的详细讨论，请参阅如何使用进度条中的使用 Swing 组件。

## `state`绑定变量

`state`绑定变量指示`SwingWorker`对象在其生命周期中的位置。绑定变量包含`SwingWorker.StateValue`类型的枚举值。可能的值包括：

`PENDING`

从对象构造到`doInBackground`调用之前的状态。

`STARTED`

从`doInBackground`调用之前的短暂时期到`done`调用之前的状态。

DONE

对象存在期间的状态。

[`SwingWorker.getState`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#getState--)返回`state`绑定变量的当前值。

## 状态方法

作为`Future`接口的一部分，还有两个方法报告后台任务的状态。正如我们在取消后台任务中看到的，如果任务已取消，`isCancelled`返回`true`。此外，如果任务已完成（无论是正常完成还是被取消），`isDone`返回`true`。
