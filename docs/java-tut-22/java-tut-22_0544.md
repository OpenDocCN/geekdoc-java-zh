# 便利实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/convenience.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/convenience.html)

这一部分描述了几个迷你实现，当你不需要它们的全部功能时，它们可能比通用实现更方便、更高效。本节中的所有实现都是通过静态工厂方法而不是`public`类提供的。

## 数组的列表视图

[`Arrays.asList`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList-T...-)方法返回其数组参数的`List`视图。对`List`的更改会写入数组，反之亦然。集合的大小与数组相同且不可更改。如果在`List`上调用`add`或`remove`方法，将导致`UnsupportedOperationException`。

这种实现的正常用法是作为基于数组和基于集合的 API 之间的桥梁。它允许你将数组传递给期望`Collection`或`List`的方法。然而，这种实现还有另一个用途。如果你需要一个固定大小的`List`，它比任何通用`List`实现更高效。这就是惯用法。

```java
List<String> list = Arrays.asList(new String[size]);

```

注意，不会保留对支持数组的引用。

## 不可变多副本列表

有时你会需要一个由多个相同元素副本组成的不可变`List`。[`Collections.nCopies`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#nCopies-int-T-)方法返回这样一个列表。这种实现有两个主要用途。第一个是初始化一个新创建的`List`；例如，假设你想要一个最初由 1,000 个`null`元素组成的`ArrayList`。下面的咒语就能实现。

```java
List<Type> list = new ArrayList<Type>(Collections.nCopies(1000, (Type)null));

```

当然，每个元素的初始值不一定是`null`。第二个主要用途是扩展现有的`List`。例如，假设你想要向`List<String>`的末尾添加 69 个字符串`"fruit bat"`的副本。不清楚为什么你想要做这样的事情，但让我们假设你确实想要。下面是如何做到的。

```java
lovablePets.addAll(Collections.nCopies(69, "fruit bat"));

```

通过使用同时接受索引和`Collection`的`addAll`形式，你可以将新元素添加到`List`的中间而不是末尾。

## 不可变单例集合

有时你会需要一个不可变的*单例*`Set`，它由一个指定的元素组成。[`Collections.singleton`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#singleton-T-)方法返回这样一个`Set`。这种实现的一个用途是从`Collection`中删除所有指定元素的出现。

```java
c.removeAll(Collections.singleton(e));

```

一个相关的惯用法是从`Map`中删除映射到指定值的所有元素。例如，假设你有一个将人员映射到他们的工作领域的`Map`  `job`  并且假设你想要消除所有律师。下面的一行代码就能完成任务。

```java
job.values().removeAll(Collections.singleton(LAWYER));

```

这种实现的另一个用途是为一个写成接受值集合的方法提供单个输入值。

## 空 Set、List 和 Map 常量

[`Collections`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html) 类提供了返回空`Set`、`List`和`Map`的方法  [`emptySet`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptySet--)、[`emptyList`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptyList--)和[`emptyMap`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptyMap--)。这些常量的主要用途是作为不提供任何值时作为值的集合的输入方法的输入，就像这个例子中一样。

```java
tourist.declarePurchases(Collections.emptySet());

```
