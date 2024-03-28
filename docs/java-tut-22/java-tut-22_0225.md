# 并发随机数

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/threadlocalrandom.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/threadlocalrandom.html)

在 JDK 7 中，[`java.util.concurrent`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html)包含一个方便的类，[`ThreadLocalRandom`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadLocalRandom.html)，适用于期望从多个线程或`ForkJoinTask`中使用随机数的应用程序。

对于并发访问，使用`ThreadLocalRandom`而不是`Math.random()`会减少争用，最终提高性能。

你只需调用`ThreadLocalRandom.current()`，然后调用其中的方法来获取一个随机数。以下是一个示例：

```java
int r = ThreadLocalRandom.current() .nextInt(4, 77);

```
