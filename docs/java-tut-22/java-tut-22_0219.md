# Executors

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/executors.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/executors.html)

在所有先前的示例中，新线程执行的任务与其`Runnable`对象定义的线程本身（由`Thread`对象定义）之间存在密切联系。这对于小型应用程序效果很好，但在大型应用程序中，将线程管理和创建与应用程序的其余部分分离是有意义的。封装这些功能的对象称为*executors*。以下小节详细描述了 executors。

+   Executor Interfaces 定义了三种 executor 对象类型。

+   Thread Pools 是最常见的 executor 实现类型。

+   Fork/Join 是一个利用多处理器的框架（JDK 7 中新增）。
