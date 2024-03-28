# 接口总结

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/summary.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/summary.html)

核心集合接口是 Java 集合框架的基础。

Java 集合框架层次结构由两个不同的接口树组成：

+   第一个树以`Collection`接口开始，该接口提供了所有集合使用的基本功能，如`add`和`remove`方法。它的子接口——`Set`、`List`和`Queue`——提供了更专门化的集合。

+   `Set`接口不允许重复元素。这对于存储诸如一副牌或学生记录之类的集合非常有用。`Set`接口有一个子接口，`SortedSet`，用于对集合中的元素进行排序。

+   `List`接口提供了一个有序的集合，用于需要精确控制每个元素插入位置的情况。您可以通过它们的确切位置从`List`中检索元素。

+   `Queue`接口允许额外的插入、提取和检查操作。`Queue`中的元素通常按照 FIFO 顺序排序。

+   `Deque`接口允许在两端进行插入、删除和检查操作。`Deque`中的元素可以同时用于 LIFO 和 FIFO。

+   第二个树以`Map`接口开始，类似于`Hashtable`将键和值进行映射。

+   `Map`的子接口`SortedMap`将其键值对按升序或按`Comparator`指定的顺序维护。

这些接口允许集合在不考虑其表示细节的情况下进行操作。
