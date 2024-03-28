# `Deque`接口

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/deque.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/deque.html)

通常发音为`deck`，双端队列是一种双端队列。双端队列是一种线性元素集合，支持在两个端点插入和移除元素。`Deque`接口比`Stack`和`Queue`更丰富的抽象数据类型，因为它同时实现了栈和队列。[`Deque`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html)接口定义了访问`Deque`实例两端元素的方法。提供了插入、移除和检查元素的方法。预定义类如[`ArrayDeque`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayDeque.html)和[`LinkedList`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)实现了`Deque`接口。

注意`Deque`接口可以同时用作后进先出栈和先进先出队列。`Deque`接口中的方法分为三部分：

## 插入

`addFirst`和`offerFirst`方法在`Deque`实例的开头插入元素。`addLast`和`offerLast`方法在`Deque`实例的末尾插入元素。当`Deque`实例的容量受限时，首选方法是`offerFirst`和`offerLast`，因为如果已满，则`addFirst`可能会失败而不抛出异常。

## 移除

`removeFirst`和`pollFirst`方法从`Deque`实例的开头移除元素。`removeLast`和`pollLast`方法从末尾移除元素。如果`Deque`为空，则`pollFirst`和`pollLast`方法返回`null`，而`removeFirst`和`removeLast`方法在`Deque`实例为空时会抛出异常。

## 检索

`getFirst`和`peekFirst`方法检索`Deque`实例的第一个元素。这些方法不会从`Deque`实例中移除值。类似地，`getLast`和`peekLast`方法检索最后一个元素。如果`deque`实例为空，则`getFirst`和`getLast`方法会抛出异常，而`peekFirst`和`peekLast`方法会返回`NULL`。

插入、移除和检索`Deque`元素的 12 种方法总结在以下表中：

**Deque 方法**

| 操作类型 | 第一个元素（`Deque`实例的开头） | 最后一个元素（`Deque`实例的末尾） |
| --- | --- | --- |
| **插入** | `addFirst(e)` `offerFirst(e)` | `addLast(e)` `offerLast(e)` |
| **移除** | `removeFirst()` `pollFirst()` | `removeLast()` `pollLast()` |
| **检查** | `getFirst()` `peekFirst()` | `getLast()` `peekLast()` |

除了这些基本方法用于插入、删除和检查`Deque`实例之外，`Deque`接口还有一些预定义方法。其中之一是`removeFirstOccurence`，此方法如果存在于`Deque`实例中，则删除指定元素的第一个出现。如果元素不存在，则`Deque`实例保持不变。另一个类似的方法是`removeLastOccurence`；此方法从`Deque`实例中删除指定元素的最后一个出现。这些方法的返回类型是`boolean`，如果元素存在于`Deque`实例中，则返回`true`。
