# 简单的后台任务

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/concurrency/simple.html`](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/simple.html)

让我们从一个非常简单但潜在耗时的任务开始。`TumbleItem` applet 加载一组用于动画的图形文件。如果图形文件是从初始线程加载的，GUI 出现之前可能会有延迟。如果图形文件是从事件分派线程加载的，GUI 可能会暂时无响应。

为了避免这些问题，`TumbleItem`从其初始线程创建并执行`SwingWorker`的一个实例。对象的`doInBackground`方法在工作线程中执行，将图像加载到`ImageIcon`数组中，并返回对其的引用。然后，在事件分派线程中执行的`done`方法调用`get`来检索此引用，并将其分配给名为`imgs`的 applet 类字段。这允许`TumbleItem`立即构造 GUI，而不必等待图像加载完成。

这里是定义和执行`SwingWorker`对象的代码。

```java
SwingWorker worker = new SwingWorker<ImageIcon[], Void>() {
    @Override
    public ImageIcon[] doInBackground() {
        final ImageIcon[] innerImgs = new ImageIcon[nimgs];
        for (int i = 0; i < nimgs; i++) {
            innerImgs[i] = loadImage(i+1);
        }
        return innerImgs;
    }

    @Override
    public void done() {
        //Remove the "Loading images" label.
        animator.removeAll();
        loopslot = -1;
        try {
            imgs = get();
        } catch (InterruptedException ignore) {}
        catch (java.util.concurrent.ExecutionException e) {
            String why = null;
            Throwable cause = e.getCause();
            if (cause != null) {
                why = cause.getMessage();
            } else {
                why = e.getMessage();
            }
            System.err.println("Error retrieving file: " + why);
        }
    }
};

```

所有`SwingWorker`的具体子类都实现`doInBackground`；`done`的实现是可选的。

请注意，`SwingWorker`是一个泛型类，有两个类型参数。第一个类型参数指定`doInBackground`的返回类型，也指定由其他线程调用以检索`doInBackground`返回的对象的`get`方法的返回类型。`SwingWorker`的第二个类型参数指定在后台任务仍处于活动状态时返回的中间结果的类型。由于此示例不返回中间结果，使用[`Void`](https://docs.oracle.com/javase/8/docs/api/java/lang/Void.html)作为占位符。

您可能会想知道设置`imgs`的代码是否过于复杂。为什么让`doInBackground`返回一个对象并使用`done`来检索它？为什么不直接让`doInBackground`直接设置`imgs`？问题在于`imgs`引用的对象是在工作线程中创建并在事件分派线程中使用的。当对象在这种方式在线程之间共享时，您必须确保在一个线程中进行的更改对另一个线程可见。使用`get`可以保证这一点，因为使用`get`会在创建`imgs`的代码和使用它的代码之间创建一个*happens before*关系。有关*happens before*关系的更多信息，请参考内存一致性错误中的并发性课程。

实际上有两种方法可以检索`doInBackground`返回的对象。

+   使用没有参数的[`SwingWorker.get`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#get--)。如果后台任务尚未完成，`get`会阻塞直到完成。

+   调用带有指示超时的参数的[`SwingWorker.get`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#get-long-java.util.concurrent.TimeUnit-)。如果后台任务尚未完成，`get`会阻塞直到完成 — 除非超时先到，此时`get`会抛出`java.util.concurrent.TimeoutException`。

在事件分发线程中调用`get`的任一重载时要小心；在`get`返回之前，不会处理任何 GUI 事件，GUI 会"冻结"。除非你确信后台任务已经完成或接近完成，否则不要调用不带参数的`get`。

要了解更多关于`TumbleItem`示例的内容，请参考如何使用 Swing 定时器中的课程使用其他 Swing 功能。
