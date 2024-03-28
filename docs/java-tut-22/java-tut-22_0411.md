# 取消后台任务

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/concurrency/cancel.html`](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/cancel.html)

要取消正在运行的后台任务，请调用[`SwingWorker.cancel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#cancel-boolean-)。任务必须配合自身的取消。有两种方式可以做到这一点：

+   通过在接收到中断时终止。这个过程在中断中有描述，在并发中有介绍。

+   通过在短时间间隔内调用[`SwingWorker.isCancelled`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#isCancelled--)。如果为这个`SwingWorker`调用了`cancel`，此方法将返回`true`。

`cancel`方法接受一个`boolean`参数。如果参数为`true`，`cancel`会向后台任务发送中断信号。无论参数是`true`还是`false`，调用`cancel`都会将对象的取消状态更改为`true`。这是`isCancelled`返回的值。一旦更改，取消状态就无法再更改。

前一节中的`Flipper`示例使用了仅检查状态的习惯用法。在`doInBackground`中的主循环在`isCancelled`返回`true`时退出。当用户点击“取消”按钮时，将触发调用带有`false`参数的`cancel`的代码。

仅检查状态的方法对于`Flipper`是有意义的，因为其`SwingWorker.doInBackground`的实现不包含可能引发`InterruptedException`的代码。要响应中断，后台任务必须在短时间间隔内调用`Thread.isInterrupted`。使用`SwingWorker.isCancelled`来实现相同的目的同样简单。

* * *

**注意：** 如果在取消后台任务后对`SwingWorker`对象调用`get`，将抛出[`java.util.concurrent.CancellationException`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CancellationException.html)。

* * *
