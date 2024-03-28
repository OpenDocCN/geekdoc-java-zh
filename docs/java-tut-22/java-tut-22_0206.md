# 内存一致性错误

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/memconsist.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/memconsist.html)

*内存一致性错误*发生在不同线程对应该是相同数据的不一致视图时。内存一致性错误的原因复杂，超出了本教程的范围。幸运的是，程序员不需要详细了解这些原因。所需的只是避免它们的策略。

避免内存一致性错误的关键在于理解*happens-before*关系。这种关系简单地保证了一个特定语句的内存写入对另一个特定语句是可见的。为了看到这一点，考虑以下示例。假设定义并初始化了一个简单的`int`字段：

```java
int counter = 0;

```

`counter`字段在两个线程 A 和 B 之间共享。假设线程 A 增加`counter`：

```java
counter++;

```

然后，不久之后，线程 B 打印出`counter`：

```java
System.out.println(counter);

```

如果这两个语句在同一个线程中执行，可以安全地假设打印出的值为"1"。但如果这两个语句在不同的线程中执行，打印出的值可能是"0"，因为不能保证线程 A 对`counter`的更改对线程 B 可见，除非程序员在这两个语句之间建立了一个 happens-before 关系。

有几种动作会创建先于关系。其中之一是同步，我们将在接下来的部分中看到。

我们已经看到了两个创建先于关系的动作。

+   当一个语句调用`Thread.start`时，与该语句具有先于关系的每个语句也与新线程执行的每个语句具有先于关系。导致创建新线程的代码的效果对新线程可见。

+   当一个线程终止并导致另一个线程中的`Thread.join`返回时，那么终止线程执行的所有语句与成功加入后面的所有语句之间存在先于关系。线程中代码的效果现在对执行加入的线程可见。

有关创建先于关系的动作列表，请参考[Java `java.util.concurrent`包的摘要页面。](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html#MemoryVisibility)。
