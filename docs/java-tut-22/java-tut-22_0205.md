# 线程干扰

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/interfere.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/interfere.html)

考虑一个简单的名为 ``Counter`` 的类

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

`Counter` 被设计成每次调用 `increment` 都会将 `c` 加 1，每次调用 `decrement` 都会从 `c` 减 1。然而，如果从多个线程引用 `Counter` 对象，线程之间的干扰可能会阻止预期的操作发生。

当两个操作在不同线程中运行，但作用于相同数据时，*干扰*就会发生。这意味着这两个操作由多个步骤组成，步骤序列会交叉。

对于 `Counter` 实例的操作似乎不可能交错，因为对 `c` 的操作都是单个简单语句。然而，即使是简单语句也可以被虚拟机翻译为多个步骤。我们不会检查虚拟机执行的具体步骤  知道单个表达式 `c++` 可以分解为三个步骤就足够了：

1.  检索当前值 `c`。

1.  递增检索到的值 1。

1.  将递增后的值存储回 `c`。

表达式 `c--` 可以以相同方式分解，只是第二步是减少而不是增加。

假设线程 A 大约在同一时间调用 `increment`，而线程 B 调用 `decrement`。如果 `c` 的初始值为 `0`，它们交错的操作可能会按照这个顺序进行：

1.  线程 A：检索 c。

1.  线程 B：检索 c。

1.  线程 A：递增检索到的值；结果为 1。

1.  线程 B：减少检索到的值；结果为 -1。

1.  线程 A：将结果存储在 c 中；c 现在为 1。

1.  线程 B：将结果存储在 c 中；c 现在为 -1。

线程 A 的结果丢失，被线程 B 覆盖。这种特定的交错只是一种可能性。在不同情况下，可能会丢失线程 B 的结果，或者根本没有错误。由于它们是不可预测的，线程干扰 bug 可能很难检测和修复。
