# 高级并发对象

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/highlevel.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/highlevel.html)

到目前为止，本课程已经专注于 Java 平台从一开始就存在的低级 API。这些 API 对于非常基本的任务是足够的，但对于更高级的任务需要更高级的构建块。这对于充分利用当今的多处理器和多核系统的大规模并发应用程序尤为重要。

在本节中，我们将介绍 Java 平台 5.0 版本引入的一些高级并发特性。这些特性大多数都是在新的`java.util.concurrent`包中实现的。Java 集合框架中还有新的并发数据结构。

+   锁对象支持简化许多并发应用程序的锁定习语。

+   执行器定义了一个用于启动和管理线程的高级 API。`java.util.concurrent`提供的执行器实现提供了适用于大规模应用程序的线程池管理。

+   并发集合使得管理大量数据集变得更加容易，并且可以大大减少同步的需求。

+   原子变量具有最小化同步和避免内存一致性错误的特性。

+   `ThreadLocalRandom`（在 JDK 7 中）提供了多线程有效生成伪随机数的功能。
