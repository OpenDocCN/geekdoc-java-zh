# 集合实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/set.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/set.html)

`Set`实现分为通用实现和特殊实现两类。

## 通用集合实现

有三种通用的[`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)实现  [`HashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html), [`TreeSet`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html), 和 [`LinkedHashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashSet.html)。在这三种中选择哪一种通常很简单。`HashSet`比`TreeSet`快得多（大部分操作的时间复杂度是常数时间对数时间），但不提供排序保证。如果你需要使用`SortedSet`接口中的操作，或者需要按值排序的迭代，使用`TreeSet`；否则，使用`HashSet`。很可能大部分时间你会使用`HashSet`。

`LinkedHashSet`在某种意义上介于`HashSet`和`TreeSet`之间。作为一个带有链表的哈希表，它提供了*插入顺序*的迭代（最近插入的到最近插入的）并且运行速度几乎和`HashSet`一样快。`LinkedHashSet`实现避免了`HashSet`提供的未指定、通常混乱的排序，而又不会增加`TreeSet`的成本。

关于`HashSet`值得注意的一点是，迭代在条目数和桶数（*容量*）的总和上是线性的。因此，选择一个初始容量太高会浪费空间和时间。另一方面，选择一个初始容量太低会浪费时间，因为每次强制增加容量时都需要复制数据结构。如果你不指定初始容量，默认值是 16。过去，选择一个质数作为初始容量有一些优势。但现在不再成立。在内部，容量总是向上舍入为 2 的幂。初始容量是通过使用`int`构造函数指定的。以下代码行分配了一个初始容量为 64 的`HashSet`。

```java
Set<String> s = new HashSet<String>(64);

```

`HashSet`类还有一个称为*负载因子*的调整参数。如果你非常关心你的`HashSet`的空间消耗，阅读`HashSet`文档以获取更多信息。否则，只需接受默认值；这几乎总是正确的做法。

如果你接受默认的负载因子但想指定一个初始容量，选择一个大约是你期望集合增长到两倍大小的数字。如果你的猜测完全错误，可能会浪费一点空间、时间或两者，但这不太可能成为一个大问题。

`LinkedHashSet`具有与`HashSet`相同的调整参数，但迭代时间不受容量影响。`TreeSet`没有调整参数。

## 特殊用途的 Set 实现

有两个特殊用途的`Set`实现  [`EnumSet`](https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html) 和 [`CopyOnWriteArraySet`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArraySet.html)。

`EnumSet`是用于枚举类型的高性能`Set`实现。枚举集合的所有成员必须是相同的枚举类型。在内部，它由一个位向量表示，通常是一个`long`。枚举集合支持对枚举类型范围的迭代。例如，给定一周中的工作日的枚举声明，你可以迭代工作日。`EnumSet`类提供了一个静态工厂，使其易于使用。

```java
    for (Day d : EnumSet.range(Day.MONDAY, Day.FRIDAY))
        System.out.println(d);

```

枚举集合也为传统位标志提供了丰富的、类型安全的替代品。

```java
    EnumSet.of(Style.BOLD, Style.ITALIC)

```

`CopyOnWriteArraySet`是一个由写时复制数组支持的`Set`实现。所有的变动操作，比如`add`、`set`和`remove`，都是通过创建数组的新副本来实现的；永远不需要加锁。即使在元素插入和删除的同时进行迭代也是安全的。与大多数`Set`实现不同，`add`、`remove`和`contains`方法所需的时间与集合大小成正比。这种实现*仅*适用于很少修改但频繁迭代的集合。它非常适合维护必须防止重复的事件处理程序列表。
