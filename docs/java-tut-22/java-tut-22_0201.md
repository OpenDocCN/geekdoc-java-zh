# 中断

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html)

*中断*是对线程的指示，告诉它应该停止当前操作并执行其他操作。程序员需要决定线程如何响应中断，但通常线程会终止。这是本课程强调的用法。

一个线程通过在要中断的线程的`Thread`对象上调用[`interrupt`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html#interrupt--)来发送中断。为了使中断机制正常工作，被中断的线程必须支持自身的中断。

## 支持中断

一个线程如何支持自身的中断？这取决于它当前正在做什么。如果线程频繁调用抛出`InterruptedException`的方法，它只需在捕获异常后从`run`方法返回。例如，假设`SleepMessages`示例中的中央消息循环在线程的`Runnable`对象的`run`方法中。那么可以修改如下以支持中断：

```java
for (int i = 0; i < importantInfo.length; i++) {
    // Pause for 4 seconds
    try {
        Thread.sleep(4000);
    } catch (InterruptedException e) {
        // We've been interrupted: no more messages.
        return;
    }
    // Print a message
    System.out.println(importantInfo[i]);
}

```

许多抛出`InterruptedException`的方法，如`sleep`，设计为在接收到中断时取消当前操作并立即返回。

如果一个线程长时间不调用抛出`InterruptedException`的方法会怎样？那么它必须定期调用`Thread.interrupted`，如果接收到中断则返回`true`。例如：

```java
for (int i = 0; i < inputs.length; i++) {
    heavyCrunch(inputs[i]);
    if (Thread.interrupted()) {
        // We've been interrupted: no more crunching.
        return;
    }
}

```

在这个简单的例子中，代码只是检测中断并在接收到中断时退出线程。在更复杂的应用程序中，抛出`InterruptedException`可能更合理：

```java
if (Thread.interrupted()) {
    throw new InterruptedException();
}

```

这使得中断处理代码可以集中在`catch`子句中。

## 中断状态标志

中断机制是通过一个称为*中断状态*的内部标志实现的。调用`Thread.interrupt`会设置这个标志。当线程通过调用静态方法`Thread.interrupted`检查中断时，中断状态会被清除。非静态的`isInterrupted`方法用于一个线程查询另一个线程的中断状态，不会改变中断状态标志。

按照惯例，任何通过抛出`InterruptedException`退出的方法在这样做时会清除中断状态。然而，另一个线程调用`interrupt`可能会立即再次设置中断状态。
