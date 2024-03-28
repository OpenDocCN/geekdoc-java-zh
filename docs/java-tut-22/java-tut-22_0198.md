# 线程对象

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/threads.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/threads.html)

每个线程都与类[`Thread`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html)的实例相关联。使用`Thread`对象创建并发应用程序有两种基本策略。

+   要直接控制线程的创建和管理，只需在应用程序需要启动异步任务时实例化`Thread`即可。

+   要将线程管理与应用程序的其余部分抽象出来，将应用程序的任务传递给一个*执行器*。

本节介绍了`Thread`对象的使用。执行器与其他高级并发对象一起讨论。
