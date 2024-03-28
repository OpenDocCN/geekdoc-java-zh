# 执行器接口

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/exinter.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/exinter.html)

`java.util.concurrent` 包定义了三个执行器接口：

+   `Executor` 是一个简单的接口，支持启动新任务。

+   `ExecutorService` 是 `Executor` 的子接口，增加了一些功能，有助于管理单个任务和执行器本身的生命周期。

+   `ScheduledExecutorService` 是 `ExecutorService` 的子接口，支持未来和/或定期执行任务。

通常，引用执行器对象的变量声明为这三种接口类型之一，而不是执行器类类型。

## `Executor` 接口

[`Executor`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executor.html) 接口提供了一个方法 `execute`，旨在成为常见线程创建习语的替代品。如果 `r` 是一个 `Runnable` 对象，`e` 是一个 `Executor` 对象，你可以替换

```java
(new Thread(r)).start();

```

with

```java
e.execute(r);

```

然而，`execute` 的定义不太具体。低级习语创建一个新线程并立即启动它。根据 `Executor` 的实现，`execute` 可能会做同样的事情，但更有可能使用现有的工作线程来运行 `r`，或者将 `r` 放入队列等待工作线程可用。（我们将在线程池部分描述工作线程。）

`java.util.concurrent` 中的执行器实现旨在充分利用更高级的 `ExecutorService` 和 `ScheduledExecutorService` 接口，尽管它们也与基本的 `Executor` 接口一起工作。

## `ExecutorService` 接口

[`ExecutorService`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html) 接口通过类似但更灵活的 `submit` 方法来补充 `execute`。与 `execute` 一样，`submit` 接受 `Runnable` 对象，但也接受 [`Callable`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Callable.html) 对象，允许任务返回一个值。`submit` 方法返回一个 [`Future`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html) 对象，用于检索 `Callable` 返回值并管理 `Callable` 和 `Runnable` 任务的状态。

`ExecutorService` 还提供了提交大量 `Callable` 对象的方法。最后，`ExecutorService` 提供了一些方法来管理执行器的关闭。为了支持立即关闭，任务应正确处理中断。

## `ScheduledExecutorService` 接口

[`ScheduledExecutorService`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ScheduledExecutorService.html) 接口通过 `schedule` 补充了其父接口 `ExecutorService` 的方法，该方法在指定延迟后执行 `Runnable` 或 `Callable` 任务。此外，该接口定义了 `scheduleAtFixedRate` 和 `scheduleWithFixedDelay`，以在定义的间隔时间内重复执行指定任务。
