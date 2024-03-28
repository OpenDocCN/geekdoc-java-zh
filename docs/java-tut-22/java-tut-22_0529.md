# SortedSet 接口

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/sorted-set.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/sorted-set.html)

[`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html) 是一个按升序维护其元素的集合，根据元素的自然顺序或在 `SortedSet` 创建时提供的 `Comparator` 进行排序。除了正常的 `Set` 操作外，`SortedSet` 接口还提供以下操作：

+   `Range view`  允许对排序集合进行任意范围操作

+   `Endpoints`  返回排序集合中的第一个或最后一个元素

+   `Comparator access`  返回用于对集合进行排序的 `Comparator`（如果有的话）

`SortedSet` 接口的代码如下。

```java
public interface SortedSet<E> extends Set<E> {
    // Range-view
    SortedSet<E> subSet(E fromElement, E toElement);
    SortedSet<E> headSet(E toElement);
    SortedSet<E> tailSet(E fromElement);

    // Endpoints
    E first();
    E last();

    // Comparator access
    Comparator<? super E> comparator();
}

```

## 集合操作

`SortedSet` 从 `Set` 继承的操作在排序集合和普通集合上的行为完全相同，但有两个例外：

+   `iterator` 操作返回的 `Iterator` 按顺序遍历排序集合。

+   `toArray` 返回的数组按顺序包含了排序后的集合元素。

尽管接口不保证，但 Java 平台的 `SortedSet` 实现的 `toString` 方法返回一个包含排序集合所有元素的字符串，按顺序排列。

## 标准构造函数

按照惯例，所有通用的 `Collection` 实现都提供一个标准的转换构造函数，接受一个 `Collection`；`SortedSet` 实现也不例外。在 `TreeSet` 中，这个构造函数创建一个根据元素的自然顺序排序的实例。这可能是一个错误。最好动态检查指定的集合是否是 `SortedSet` 实例，如果是，则根据相同的标准（比较器或自然顺序）对新的 `TreeSet` 进行排序。因为 `TreeSet` 采取了它的方法，它还提供一个接受 `SortedSet` 的构造函数，并返回一个包含相同元素并根据相同标准排序的新 `TreeSet`。请注意，参数的编译时类型，而不是运行时类型，决定调用这两个构造函数中的哪一个（以及是否保留排序标准）。

`SortedSet` 实现通常还提供一个构造函数，接受一个 `Comparator` 并返回一个根据指定 `Comparator` 排序的空集合。如果传递 `null` 给这个构造函数，它将返回一个根据元素的自然顺序排序的集合。

## 范围视图操作

`range-view`操作在某种程度上类似于`List`接口提供的操作，但有一个重大区别。排序集的范围视图即使在直接修改支持的排序集的情况下仍然有效。这是因为排序集的范围视图的端点是元素空间中的绝对点，而不是备份集合中的特定元素，这对于列表是成立的。排序集的`range-view`实际上只是窗口，显示在元素空间的指定部分中集合的任何部分。对排序集的`range-view`的更改会写回到支持的排序集中，反之亦然。因此，长时间使用排序集上的`range-view`是可以的，不像列表上的`range-view`那样。

排序集提供三种`range-view`操作。第一种是`subSet`，类似于`subList`，需要两个端点。端点不是索引，而是对象，并且必须与排序集中的元素可比，使用`Set`的`Comparator`或其元素的自然排序，取决于`Set`用于对自身排序的方式。与`subList`类似，范围是半开的，包括其低端点但不包括高端点。

因此，下面这行代码告诉你在名为`dictionary`的字符串`SortedSet`中包含多少个介于`"doorbell"`和`"pickle"`之间的单词，包括`"doorbell"`但不包括`"pickle"`。

```java
int count = dictionary.subSet("doorbell", "pickle").size();

```

以类似的方式，以下一行代码可以删除所有以字母`f`开头的元素。

```java
dictionary.subSet("f", "g").clear();

```

类似的技巧可以用来打印一个表格，告诉你每个字母开头的单词有多少个。

```java
for (char ch = 'a'; ch <= 'z'; ) {
    String from = String.valueOf(ch++);
    String to = String.valueOf(ch);
    System.out.println(from + ": " + dictionary.subSet(from, to).size());
}

```

假设你想查看一个*闭区间*，其中包含两个端点，而不是一个开区间。如果元素类型允许计算元素空间中给定值的后继，只需请求从`lowEndpoint`到`successor(highEndpoint)`的`subSet`。虽然这并不是完全明显的，但在`String`的自然排序中，字符串`s`的后继是`s + "\0"`，也就是在`s`后附加一个`null`字符。

因此，以下一行代码告诉你在字典中包含多少个介于`"doorbell"`和`"pickle"`之间的单词，包括`doorbell` *和* `pickle`。

```java
count = dictionary.subSet("doorbell", "pickle\0").size();

```

类似的技巧可以用来查看一个*开区间*，其中不包含任何端点。从`lowEndpoint`到`highEndpoint`的开区间视图是从`successor(lowEndpoint)`到`highEndpoint`的半开区间。使用以下代码计算介于`"doorbell"`和`"pickle"`之间的单词数量，不包括两者。

```java
count = dictionary.subSet("doorbell\0", "pickle").size();

```

`SortedSet` 接口包含两个更多的 `range-view` 操作  `headSet` 和 `tailSet`，两者都接受一个 `Object` 参数。前者返回一个视图，显示了支持 `SortedSet` 的初始部分，直到但不包括指定的对象。后者返回一个视图，显示了支持 `SortedSet` 的最终部分，从指定的对象开始，一直到支持 `SortedSet` 的末尾。因此，以下代码允许您将字典视为两个不相交的 `卷`（`a-m` 和 `n-z`）。

```java
SortedSet<String> volume1 = dictionary.headSet("n");
SortedSet<String> volume2 = dictionary.tailSet("n");

```

## 端点操作

`SortedSet` 接口包含返回排序集合中第一个和最后一个元素的操作，分别称为 `first` 和 `last`。除了它们的明显用途外，`last` 还允许解决 `SortedSet` 接口中的一个缺陷。您希望对 `SortedSet` 进行的一件事是进入 `Set` 的内部并向前或向后迭代。从内部向前迭代很容易：只需获取一个 `tailSet` 并对其进行迭代。不幸的是，向后迭代没有简单的方法。

以下习语获取了元素空间中小于指定对象 `o` 的第一个元素。

```java
Object predecessor = ss.headSet(o).last();

```

这是从排序集合内部的某一点向后移动一个元素的好方法。可以重复应用它来向后迭代，但这非常低效，每返回一个元素都需要查找。

## 比较器访问器

`SortedSet` 接口包含一个名为 `comparator` 的访问器方法，返回用于对集合进行排序的 `Comparator`，如果集合根据其元素的 *自然顺序* 进行排序，则返回 `null`。提供此方法是为了可以将排序集合复制到具有相同顺序的新排序集合中。它被描述为 `SortedSet` 构造函数使用的 先前。
