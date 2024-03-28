# 原子变量

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/atomicvars.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/atomicvars.html)

[`java.util.concurrent.atomic`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/package-summary.html)包定义了支持单个变量上原子操作的类。所有类都有类似于对`volatile`变量进行读取和写入的`get`和`set`方法。也就是说，`set`与同一变量上的任何后续`get`之间存在 happens-before 关系。原子`compareAndSet`方法也具有这些内存一致性特性，整数原子变量适用的简单原子算术方法也是如此。

要了解这个包可能如何使用，让我们回到最初用来演示线程干扰的`Counter`类：

```java

class Counter {
    private int c = 0;

    public void increment() {
        c++;
    }

    public void decrement() {
        c--;
    }

    public int value() {
        return c;
    }

}

```

使`Counter`免受线程干扰的一种方法是使其方法同步，就像`SynchronizedCounter`中那样：

```java

class SynchronizedCounter {
    private int c = 0;

    public synchronized void increment() {
        c++;
    }

    public synchronized void decrement() {
        c--;
    }

    public synchronized int value() {
        return c;
    }

}

```

对于这个简单的类，同步是一个可接受的解决方案。但对于一个更复杂的类，我们可能希望避免不必要同步的活跃度影响。用`AtomicInteger`替换`int`字段可以让我们在不使用同步的情况下防止线程干扰，就像`AtomicCounter`中那样：

```java

import java.util.concurrent.atomic.AtomicInteger;

class AtomicCounter {
    private AtomicInteger c = new AtomicInteger(0);

    public void increment() {
        c.incrementAndGet();
    }

    public void decrement() {
        c.decrementAndGet();
    }

    public int value() {
        return c.get();
    }

}

```
