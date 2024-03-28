# Worker Threads 和 SwingWorker

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/concurrency/worker.html`](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/worker.html)

当 Swing 程序需要执行长时间运行的任务时，通常会使用其中一个 *worker 线程*，也称为 *后台线程*。在 worker 线程上运行的每个任务都由一个 [`javax.swing.SwingWorker`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html) 实例表示。`SwingWorker` 本身是一个抽象类；您必须定义一个子类才能创建一个 `SwingWorker` 对象；匿名内部类通常用于创建非常简单的 `SwingWorker` 对象。

`SwingWorker` 提供了许多通信和控制功能：

+   `SwingWorker` 子类可以定义一个方法 `done`，当后台任务完成时，该方法会自动在事件分发线程上调用。

+   `SwingWorker` 实现了 [`java.util.concurrent.Future`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html)。这个接口允许后台任务向其他线程提供返回值。此接口中的其他方法允许取消后台任务，并发现后台任务是否已完成或已取消。

+   后台任务可以通过调用 `SwingWorker.publish` 提供中间结果，导致从事件分发线程调用 `SwingWorker.process`。

+   后台任务可以定义绑定属性。对这些属性的更改会触发事件，导致事件处理方法在事件分发线程上被调用。

这些功能将在以下小节中讨论。

* * *

**注意：**

`javax.swing.SwingWorker` 类是在 Java SE 6 中添加到 Java 平台的。在此之前，另一个类，也称为 `SwingWorker`，被广泛用于一些相同的目的。旧的 `SwingWorker` 不是 Java 平台规范的一部分，也不作为 JDK 的一部分提供。

新的 `javax.swing.SwingWorker` 是一个全新的类。其功能不是旧的 `SwingWorker` 的严格超集。这两个类中具有相同功能的方法没有相同的名称。此外，旧的 `SwingWorker` 类的实例是可重用的，而每个新的后台任务都需要一个新的 `javax.swing.SwingWorker` 实例。

在 Java 教程中，任何提到 `SwingWorker` 现在都指的是 `javax.swing.SwingWorker`。

* * *
