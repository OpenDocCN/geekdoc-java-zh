# Map 实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/map.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/map.html)

`Map`实现分为通用、特殊和并发实现。

## 通用 Map 实现

三种通用[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)实现是[`HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)、[`TreeMap`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)和[`LinkedHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html)。如果需要`SortedMap`操作或基于键排序的`Collection`-view 迭代，请使用`TreeMap`；如果希望最大速度且不关心迭代顺序，请使用`HashMap`；如果希望接近`HashMap`性能且插入顺序迭代，请使用`LinkedHashMap`。在这方面，`Map`的情况类似于`Set`。同样，Set Implementations 部分中的其他所有内容也适用于`Map`实现。

`LinkedHashMap`提供了两个`LinkedHashSet`不可用的功能。当您创建一个`LinkedHashMap`时，您可以根据键访问而不是插入对其进行排序。换句话说，仅查找与键关联的值会将该键移到地图的末尾。此外，`LinkedHashMap`提供了`removeEldestEntry`方法，可以被覆盖以在向地图添加新映射时自动实施删除过时映射的策略。这使得实现自定义缓存非常容易。

例如，这个覆盖将允许地图增长到多达 100 个条目，然后每次添加新条目时都会删除最老的条目，保持 100 个条目的稳定状态。

```java
private static final int MAX_ENTRIES = 100;

protected boolean removeEldestEntry(Map.Entry eldest) {
    return size() > MAX_ENTRIES;
}

```

## 特殊用途 Map 实现

有三种特殊用途的 Map 实现  [`EnumMap`](https://docs.oracle.com/javase/8/docs/api/java/util/EnumMap.html)、[`WeakHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/WeakHashMap.html)和[`IdentityHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/IdentityHashMap.html)。`EnumMap`，内部实现为`array`，是用于枚举键的高性能`Map`实现。此实现将`Map`接口的丰富性和安全性与接近数组的速度结合在一起。如果要将枚举映射到值，应始终优先使用`EnumMap`而不是数组。

`WeakHashMap` 是`Map`接口的一个实现，只存储对其键的弱引用。只存储弱引用允许在其键不再在`WeakHashMap`之外被引用时，键值对可以被垃圾回收。这个类提供了利用弱引用功能的最简单方法。它对于实现“注册表样”数据结构非常有用，其中一个条目的实用性在其键不再被任何线程引用时消失。

`IdentityHashMap` 是基于哈希表的基于身份的`Map`实现。这个类对于保持拓扑结构的对象图转换非常有用，比如序列化或深拷贝。为了执行这样的转换，你需要维护一个基于身份的“节点表”，用于跟踪哪些对象已经被看到。基于身份的映射也用于在动态调试器和类似系统中维护对象到元信息的映射。最后，基于身份的映射对于阻止由于故意扭曲的`equals`方法而导致的“欺骗攻击”非常有用，因为`IdentityHashMap`永远不会在其键上调用`equals`方法。这个实现的一个额外好处是它很快。

## 并发映射实现

[`java.util.concurrent`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html) 包含了[`ConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html) 接口，它通过原子的`putIfAbsent`、`remove`和`replace`方法扩展了`Map`，以及该接口的[`ConcurrentHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html) 实现。

`ConcurrentHashMap` 是一个高度并发、高性能的哈希表实现。在执行检索操作时，此实现永远不会阻塞，并允许客户端选择更新的并发级别。它旨在作为`Hashtable`的一个可替换项：除了实现`ConcurrentMap`外，它还支持所有`Hashtable`特有的传统方法。再次强调，如果你不需要传统操作，请小心使用`ConcurrentMap`接口来操作它。
