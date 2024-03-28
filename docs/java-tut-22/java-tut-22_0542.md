# Deque 实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/deque.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/deque.html)

`Deque` 接口，发音为*"deck"*, 代表双端队列。`Deque` 接口可以被实现为各种类型的`Collections`。`Deque` 接口的实现被分为通用和并发实现。

## 通用 Deque 实现

通用实现包括`LinkedList` 和 `ArrayDeque` 类。`Deque` 接口支持在两端插入、删除和检索元素。[`ArrayDeque`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayDeque.html) 类是`Deque` 接口的可调整大小数组实现，而[`LinkedList`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) 类是列表实现。

`Deque` 接口中的基本插入、删除和检索操作有`addFirst`、`addLast`、`removeFirst`、`removeLast`、`getFirst` 和 `getLast`。`addFirst` 方法在头部添加元素，而`addLast` 方法在`Deque` 实例的尾部添加元素。

`LinkedList` 实现比`ArrayDeque` 实现更灵活。`LinkedList` 实现了所有可选的列表操作。`LinkedList` 实现允许`null`元素，但`ArrayDeque` 实现不允许。

就效率而言，`ArrayDeque` 在两端的添加和删除操作上比`LinkedList` 更高效。在迭代过程中，`LinkedList` 实现中最好的操作是删除当前元素。`LinkedList` 实现不是理想的迭代结构。

`LinkedList` 实现比`ArrayDeque` 实现消耗更多内存。对于`ArrayDeque` 实例的遍历，可以使用以下任意一种：

### foreach

`foreach` 是一种快速且适用于各种列表的方法。

```java

ArrayDeque<String> aDeque = new ArrayDeque<String>();

. . .
for (String str : aDeque) {
    System.out.println(str);
}

```

### 迭代器

`Iterator` 可用于所有类型的数据的所有类型列表的正向遍历。

```java
ArrayDeque<String> aDeque = new ArrayDeque<String>();
. . .
for (Iterator<String> iter = aDeque.iterator(); iter.hasNext();  ) {
    System.out.println(iter.next());
}

```

`ArrayDeque` 类在本教程中用于实现`Deque` 接口。本教程中使用的示例完整代码在``ArrayDequeSample``中可用。`LinkedList` 和 `ArrayDeque` 类都不支持多线程并发访问。

## 并发 Deque 实现

[`LinkedBlockingDeque`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/LinkedBlockingDeque.html) 类是`Deque` 接口的并发实现。如果双端队列为空，则`takeFirst` 和 `takeLast` 等方法会等待直到元素变为可用，然后检索并移除相同的元素。
