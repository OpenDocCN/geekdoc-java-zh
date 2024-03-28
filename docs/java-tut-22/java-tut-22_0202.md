# 加入

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/join.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/join.html)

`join`方法允许一个线程等待另一个线程的完成。如果`t`是一个当前正在执行的线程的`Thread`对象，

```java
t.join();

```

会导致当前线程暂停执行，直到`t`的线程终止。`join`的重载允许程序员指定等待时间。然而，与`sleep`一样，`join`依赖于操作系统的时间控制，因此你不应该假设`join`会等待与你指定的时间完全相同。

像`sleep`一样，`join`在收到`InterruptedException`时会退出。
