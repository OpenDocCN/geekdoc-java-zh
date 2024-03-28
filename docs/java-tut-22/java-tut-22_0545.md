# 实现摘要

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/summary.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/summary.html)

实现是用于存储集合的数据对象，这些对象实现了接口课程中描述的接口。

Java 集合框架提供了几个核心接口的通用实现：

+   对于`Set`接口，`HashSet`是最常用的实现。

+   对于`List`接口，`ArrayList`是最常用的实现。

+   对于`Map`接口，`HashMap`是最常用的实现。

+   对于`Queue`接口，`LinkedList`是最常用的实现。

+   对于`Deque`接口，`ArrayDeque`是最常用的实现。

每个通用实现提供其接口中包含的所有可选操作。

Java 集合框架还提供了几个特殊用途的实现，用于需要非标准性能、使用限制或其他异常行为的情况。

`java.util.concurrent`包含几个集合实现，这些实现是线程安全的，但不受单个排他锁的控制。

`Collections`类（与`Collection`接口相对），提供了在集合上操作或返回集合的静态方法，这些方法被称为包装器实现。

最后，还有几个便利实现，当您不需要它们的全部功能时，这些便利实现可能比通用实现更有效。这些便利实现通过静态工厂方法提供。
