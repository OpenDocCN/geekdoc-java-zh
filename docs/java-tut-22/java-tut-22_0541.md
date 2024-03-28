# 队列实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/queue.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/queue.html)

`Queue` 实现分为通用和并发实现。

## 通用队列实现

如前一节所述，`LinkedList` 实现了 `Queue` 接口，为 `add`、`poll` 等提供先进先出（FIFO）队列操作。

[`PriorityQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/PriorityQueue.html) 类是基于 *堆* 数据结构的优先队列。此队列根据在构造时指定的顺序对元素进行排序，可以是元素的自然顺序或由显式 `Comparator` 强加的顺序。

队列检索操作  `poll`、`remove`、`peek` 和 `element`  访问队列头部的元素。队列的 *头部* 是相对于指定顺序的最小元素。如果多个元素具有最小值，则头部是这些元素之一；平局将被任意打破。

`PriorityQueue` 及其迭代器实现了 `Collection` 和 `Iterator` 接口的所有可选方法。在 `iterator` 方法中提供的迭代器不能保证以任何特定顺序遍历 `PriorityQueue` 的元素。对于有序遍历，请考虑使用 `Arrays.sort(pq.toArray())`。

## 并发队列实现

`java.util.concurrent` 包含一组同步的 `Queue` 接口和类。[`BlockingQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html) 扩展了 `Queue`，具有在检索元素时等待队列变得非空以及在存储元素时等待队列中有空间可用的操作。该接口由以下类实现：

+   [`LinkedBlockingQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/LinkedBlockingQueue.html)  由链表节点支持的可选有界 FIFO 阻塞队列

+   [`ArrayBlockingQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ArrayBlockingQueue.html)  由数组支持的有界 FIFO 阻塞队列

+   [`PriorityBlockingQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/PriorityBlockingQueue.html)  由堆支持的无界阻塞优先级队列

+   [`DelayQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/DelayQueue.html)  由堆支持的基于时间的调度队列

+   [`SynchronousQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/SynchronousQueue.html)  使用 `BlockingQueue` 接口的简单会合机制

在 JDK 7 中，[`TransferQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/TransferQueue.html)是一个专门的`BlockingQueue`，其中向队列添加元素的代码可以选择等待（阻塞），直到另一个线程中的代码检索元素。`TransferQueue`只有一个实现：

+   [`LinkedTransferQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/LinkedTransferQueue.html)  基于链表节点的无界`TransferQueue`
