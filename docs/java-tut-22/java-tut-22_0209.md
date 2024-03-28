# 原子访问

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/atomic.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/atomic.html)

在编程中，*原子*操作是一种有效地一次性完成的操作。原子操作不能在中途停止：它要么完全发生，要么根本不发生。在原子操作完成之前，不会看到任何副作用。

我们已经看到增量表达式，比如`c++`，并不描述原子操作。即使是非常简单的表达式也可以定义可以分解为其他操作的复杂操作。然而，有一些操作是可以指定为原子操作的：

+   对于引用变量和大多数基本变量（除了`long`和`double`之外的所有类型），读取和写入是原子的。

+   对于所有声明为`volatile`的变量，读取和写入都是原子的（包括`long`和`double`变量）。

原子操作不能交错，因此可以在不担心线程干扰的情况下使用它们。然而，这并不消除同步原子操作的所有需求，因为内存一致性错误仍然可能发生。使用`volatile`变量可以减少内存一致性错误的风险，因为对`volatile`变量的任何写入都会与随后对该变量的读取建立 happens-before 关系。这意味着对`volatile`变量的更改始终对其他线程可见。更重要的是，这也意味着当线程读取`volatile`变量时，它不仅看到`volatile`的最新更改，还看到导致更改的代码的副作用。

使用简单的原子变量访问比通过同步代码访问这些变量更有效，但需要程序员更加小心，以避免内存一致性错误。额外的努力是否值得取决于应用程序的大小和复杂性。

[`java.util.concurrent`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html)包中的一些类提供了不依赖于同步的原子方法。我们将在高级并发对象部分讨论它们。
