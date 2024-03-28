# 课程：Swing 中的并发

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/concurrency/index.html`](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/index.html)

示例索引

本课程讨论了并发如何应用于 Swing 编程。它假定您已经熟悉 Essential Java Classes 路径中 Concurrency 课程的内容。

慎重使用并发对于 Swing 程序员尤为重要。一个编写良好的 Swing 程序利用并发创建一个用户界面，永远不会“冻结” —— 无论正在做什么，程序始终对用户交互做出响应。为了创建一个响应灵敏的程序，程序员必须了解 Swing 框架如何使用线程。

Swing 程序员处理以下类型的线程：

+   *初始线程*，执行初始应用程序代码的线程。

+   *事件分发线程*，执行所有事件处理代码。大多数与 Swing 框架交互的代码也必须在此线程上执行。

+   *工作线程*，也称为*后台线程*，执行耗时的后台任务。

程序员不需要提供明确创建这些线程的代码：它们由运行时或 Swing 框架提供。程序员的工作是利用这些线程创建一个响应灵敏、易于维护的 Swing 程序。

与在 Java 平台上运行的任何其他程序一样，Swing 程序可以使用并发课程中描述的工具创建额外的线程和线程池。但是对于基本的 Swing 程序，这里描述的线程就足够了。

本课程依次讨论了三种线程。工作线程需要更多讨论，因为在其上运行的任务是使用`javax.swing.SwingWorker`创建的。这个类具有许多有用的功能，包括工作线程任务与其他线程上的任务之间的通信和协调。
