# 列表实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/list.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/list.html)

`List`实现分为通用和特殊用途的实现。

## 通用列表实现

有两种通用的[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)实现——[`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)和[`LinkedList`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)。大多数情况下，你可能会使用`ArrayList`，它提供常数时间的位置访问，并且非常快速。它不必为`List`中的每个元素分配一个节点对象，并且在需要同时移动多个元素时可以利用`System.arraycopy`。将`ArrayList`视为没有同步开销的`Vector`。

如果你经常在`List`的开头添加元素或者迭代`List`以删除其内部的元素，你应该考虑使用`LinkedList`。这些操作在`LinkedList`中需要常数时间，在`ArrayList`中需要线性时间。但你会在性能上付出很大的代价。在`LinkedList`中，位置访问需要线性时间，在`ArrayList`中需要常数时间。此外，`LinkedList`的常数因子要糟糕得多。如果你认为你想使用`LinkedList`，在做出选择之前用`LinkedList`和`ArrayList`测量你的应用程序的性能；`ArrayList`通常更快。

`ArrayList`有一个调整参数——*初始容量*，指的是`ArrayList`在增长之前可以容纳的元素数量。`LinkedList`没有调整参数，但有七个可选操作，其中之一是`clone`。另外六个是`addFirst`、`getFirst`、`removeFirst`、`addLast`、`getLast`和`removeLast`。`LinkedList`还实现了`Queue`接口。

## 特殊用途的列表实现

[`CopyOnWriteArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArrayList.html)是一个由写入时复制数组支持的`List`实现。这种实现与`CopyOnWriteArraySet`类似。即使在迭代期间，也不需要同步，并且迭代器永远不会抛出`ConcurrentModificationException`。这种实现非常适合维护事件处理程序列表，其中变化不频繁，遍历频繁且可能耗时。

如果你需要同步，`Vector`比使用`Collections.synchronizedList`同步的`ArrayList`稍快一些。但`Vector`有很多遗留操作，所以一定要小心，始终使用`List`接口操作`Vector`，否则你将无法在以后替换实现。

如果你的`List`大小是固定的  也就是说，你永远不会使用`remove`、`add`或者除了`containsAll`之外的任何批量操作  那么你有第三个选项，绝对值得考虑。查看方便实现部分的`Arrays.asList`获取更多信息。
