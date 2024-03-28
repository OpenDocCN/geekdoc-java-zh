# 线程池

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/pools.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/pools.html)

`java.util.concurrent` 中的大多数执行器实现使用*线程池*，其中包含*工作线程*。这种类型的线程与它执行的`Runnable`和`Callable`任务分开存在，并经常用于执行多个任务。

使用工作线程可以最小化由于线程创建而产生的开销。线程对象使用大量内存，在大规模应用程序中，分配和释放许多线程对象会产生显著的内存管理开销。

一种常见的线程池类型是*固定线程池*。这种类型的池始终有指定数量的线程在运行；如果某个线程在仍在使用时被终止，它将自动被新线程替换。任务通过内部队列提交到池中，当活动任务多于线程时，队列会保存额外的任务。

使用固定线程池的一个重要优势是应用程序在使用它时*优雅降级*。要理解这一点，考虑一个 Web 服务器应用程序，其中每个 HTTP 请求都由一个单独的线程处理。如果应用程序只是为每个新的 HTTP 请求创建一个新线程，并且系统接收到的请求多于它立即处理的能力，当所有这些线程的开销超过系统容量时，应用程序将突然停止响应*所有*请求。通过限制可以创建的线程数量，应用程序将不会像请求进来那样快速地为 HTTP 请求提供服务，但它将以系统能够维持的速度为它们提供服务。

创建使用固定线程池的执行器的简单方法是在[`java.util.concurrent.Executors`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html)中调用[`newFixedThreadPool`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html#newFixedThreadPool-int-)工厂方法。该类还提供以下工厂方法：

+   [`newCachedThreadPool`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html#newCachedThreadPool-int-)方法创建一个具有可扩展线程池的执行器。此执行器适用于启动许多短暂任务的应用程序。

+   [`newSingleThreadExecutor`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html#newSingleThreadExecutor-int-)方法创建一个一次执行一个任务的执行器。

+   几个工厂方法是上述执行器的`ScheduledExecutorService`版本。

如果上述工厂方法提供的任何执行器都不符合您的需求，构造[`java.util.concurrent.ThreadPoolExecutor`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadPoolExecutor.html)或[`java.util.concurrent.ScheduledThreadPoolExecutor`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ScheduledThreadPoolExecutor.html)的实例将为您提供额外的选项。
