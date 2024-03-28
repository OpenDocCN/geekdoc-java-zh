# 教训：实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/index.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/index.html)

实现是用于存储集合的数据对象，实现了接口部分中描述的接口。本课程描述了以下类型的实现：

+   **通用实现**是最常用的实现，设计用于日常使用。它们在标题为通用实现的表中总结。

+   **特殊用途实现**设计用于特殊情况，并显示非标准性能特征、使用限制或行为。

+   **并发实现**旨在支持高并发性，通常以牺牲单线程性能为代价。这些实现是`java.util.concurrent`包的一部分。

+   **包装实现**通常与其他类型的实现结合使用，通常是通用实现，以提供附加或受限功能。

+   **便利实现**是迷你实现，通常通过静态工厂方法提供，为特殊集合提供方便、高效的替代方案（例如，单例集合）。

+   **抽象实现**是骨架实现，有助于构建自定义实现，稍后在自定义集合实现部分中描述。这是一个高级主题，不是特别困难，但相对较少的人会需要这样做。

通用实现总结如下表所示。

**通用实现**

| 接口 | 哈希表实现 | 可调整大小数组实现 | 树实现 | 链表实现 | 哈希表 + 链表实现 |
| --- | --- | --- | --- | --- | --- |
| `Set` | `HashSet` |   | `TreeSet` |   | `LinkedHashSet` |
| `List` |   | `ArrayList` |   | `LinkedList` |   |
| `Queue` |   |   |   |   |   |
| `Deque` |   | `ArrayDeque` |   | `LinkedList` |   |
| `Map` | `HashMap` |   | `TreeMap` |   | `LinkedHashMap` |

正如您从表中所看到的，Java 集合框架提供了几个通用实现的[`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)、[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)和[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)接口。在每种情况下，一个实现——[`HashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)、[`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)和[`HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)——显然是大多数应用程序中要使用的实现，其他条件相等。请注意，[`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html)和[`SortedMap`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)接口在表中没有行。每个接口都有一个实现（[`TreeSet`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html)和[`TreeMap`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)），并列在`Set`和`Map`行中。有两个通用的`Queue`实现——[`LinkedList`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)，也是`List`实现，和[`PriorityQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/PriorityQueue.html)，在表中被省略。这两个实现提供非常不同的语义：`LinkedList`提供 FIFO 语义，而`PriorityQueue`根据其值对元素进行排序。

每个通用实现都提供其接口中包含的所有可选操作。所有允许`null`元素、键和值。没有同步（线程安全）。所有都有*快速失败迭代器*，在迭代期间检测到非法并发修改，并快速干净地失败，而不是在未来的某个不确定时间冒险出现任意、非确定性的行为。所有都是`Serializable`，并支持公共`clone`方法。

这些实现不同步的事实代表了与过去的断裂：传统集合`Vector`和`Hashtable`是同步的。采取当前方法是因为在同步没有好处时集合经常被使用。这些用途包括单线程使用、只读使用以及作为执行自身同步的较大数据对象的一部分使用。一般来说，良好的 API 设计实践是不让用户为他们不使用的功能付费。此外，不必要的同步可能在某些情况下导致死锁。

如果你需要线程安全的集合，包装器实现部分描述的同步包装器允许*任何*集合转换为同步集合。因此，同步对于通用实现是可选的，而对于传统实现是强制的。此外，`java.util.concurrent`包提供了`BlockingQueue`接口的并发实现，它扩展了`Queue`，以及`ConcurrentMap`接口的并发实现，它扩展了`Map`。这些实现比单纯的同步实现具有更高的并发性。

通常情况下，你应该考虑接口，*而不是*实现。这就是为什么本节中没有编程示例。在很大程度上，实现的选择只影响性能。如接口部分所述，首选的风格是在创建`Collection`时选择一个实现，并立即将新集合分配给相应接口类型的变量（或将集合传递给期望接口类型参数的方法）。通过这种方式，程序不会依赖于给定实现中添加的任何方法，使程序员可以自由更改实现，只要性能或行为细节需要。

接下来的部分简要讨论了实现。使用诸如*常数时间*、*对数*、*线性*、*n log(n)*和*二次*等词语描述了实现的性能，以指代执行操作的时间复杂度的渐近上界。所有这些都是相当复杂的术语，如果你不知道它们的含义也没关系。如果你对更多信息感兴趣，请参考任何一本优秀的算法教材。要记住的一件事是，这种性能指标有其局限性。有时，名义上更慢的实现可能更快。如果有疑问，就要测量性能！
