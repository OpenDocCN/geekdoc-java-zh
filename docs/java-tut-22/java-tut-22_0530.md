# 排序地图接口

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/sorted-map.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/sorted-map.html)

一个[`SortedMap`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)是一个维护其条目按升序排列的[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)，根据键的自然顺序排序，或者根据在创建`SortedMap`时提供的`Comparator`排序。自然排序和`Comparator`在对象排序部分讨论。`SortedMap`接口提供了常规`Map`操作以及以下操作：

+   `Range view`  在排序地图上执行任意范围操作

+   `Endpoints`  返回排序地图中的第一个或最后一个键

+   `Comparator access`  返回用于对地图进行排序的`Comparator`（如果有的话）。

以下接口是[`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html)的`Map`模拟。

```java
public interface SortedMap<K, V> extends Map<K, V>{
    Comparator<? super K> comparator();
    SortedMap<K, V> subMap(K fromKey, K toKey);
    SortedMap<K, V> headMap(K toKey);
    SortedMap<K, V> tailMap(K fromKey);
    K firstKey();
    K lastKey();
}

```

## 地图操作

`SortedMap`从`Map`继承的操作在排序地图和普通地图上的行为相同，有两个例外：

+   任何排序地图的`Collection`视图上的`iterator`操作返回的`Iterator`按顺序遍历集合。

+   `Collection`视图的`toArray`操作返回的数组按顺序包含键、值或条目。

虽然接口不能保证，但 Java 平台所有`SortedMap`实现中`Collection`视图的`toString`方法返回一个字符串，其中包含视图中的所有元素，按顺序排列。

## 标准构造函数

按照惯例，所有通用`Map`实现都提供一个标准转换构造函数，接受一个`Map`；`SortedMap`实现也不例外。在`TreeMap`中，这个构造函数创建一个根据其键的自然顺序排序其条目的实例。这可能是一个错误。最好动态检查指定的`Map`实例是否是`SortedMap`，如果是，则根据相同的标准（比较器或自然顺序）对新地图进行排序。因为`TreeMap`采取了它的方法，它还提供了一个接受`SortedMap`的构造函数，并返回一个包含与给定`SortedMap`相同映射的新`TreeMap`，根据相同标准排序。请注意，参数的编译时类型，而不是运行时类型，决定了是否优先调用`SortedMap`构造函数而不是普通的`map`构造函数。

按照惯例，`SortedMap`实现还提供一个接受`Comparator`的构造函数，并返回根据指定`Comparator`排序的空地图。如果将`null`传递给此构造函数，则返回一个根据其键的自然顺序对其映射进行排序的`Map`。

## 与 SortedSet 的比较

因为这个接口是`SortedSet`的精确`Map`模拟，所以在 SortedSet 接口部分中的所有习语和代码示例都适用于`SortedMap`，只需进行微不足道的修改。
