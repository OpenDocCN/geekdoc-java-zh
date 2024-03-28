# 教训：自定义集合实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/custom-implementations/index.html`](https://docs.oracle.com/javase/tutorial/collections/custom-implementations/index.html)

许多程序员永远不需要实现自己的`Collection`类。您可以通过使用本章前面描述的实现来走得很远。但是，总有一天您可能想要编写自己的实现。借助 Java 平台提供的抽象实现，这样做相当容易。在讨论*如何*编写实现之前，让我们讨论一下为什么您可能想要编写一个。

## 编写实现的原因

以下列表说明了您可能想要实现的自定义`Collection`类型。这并不是详尽无遗的：

+   **持久性**：所有内置的`Collection`实现都驻留在主内存中，并在程序退出时消失。如果您希望在下次程序启动时仍然存在的集合，可以通过在外部数据库上构建一个薄层来实现它。这样的集合可能可以被多个程序同时访问。

+   **特定应用**：这是一个非常广泛的类别。一个例子是包含实时遥测数据的不可修改的`Map`。键可以表示位置，值可以根据`get`操作从这些位置的传感器中读取。

+   **高性能，特定用途**：许多数据结构利用受限使用来提供比通用实现更好的性能。例如，考虑一个包含长时间相同元素值的`List`。这样的列表在文本处理中经常出现，可以进行*运行长度编码*  运行可以表示为包含重复元素和连续重复次数的单个对象。这个例子很有趣，因为它在性能方面进行了权衡：它需要更少的空间但比`ArrayList`需要更多的时间。

+   **高性能，通用用途**：Java 集合框架的设计者试图为每个接口提供最佳的通用实现，但是可以使用许多许多数据结构，并且每天都会有新的数据结构被发明。也许您可以想出更快的解决方案！

+   **增强功能**：假设您需要一个高效的包实现（也称为*multiset*）：一个`Collection`，它在允许重复元素的同时提供常数时间的包含性检查。在`HashMap`之上实现这样一个集合是相当简单的。

+   **便利性**：您可能希望提供超出 Java 平台提供的便利的其他实现。例如，您可能经常需要表示一系列`Integer`的连续范围的`List`实例。

+   **适配器**：假设您正在使用具有自己的特定集合 API 的旧 API。您可以编写一个适配器实现，使这些集合能够在 Java 集合框架中运行。*适配器实现*是一个薄膜，包装一种类型的对象，并通过将对后者类型的操作转换为对前者类型的操作来使其行为类似于另一种类型的对象。

## 如何编写自定义实现

编写自定义实现出人意料地容易。Java 集合框架提供了专门设计用于促进自定义实现的抽象实现。我们将从以下`Arrays.asList`实现的示例开始。

```java
public static <T> List<T> asList(T[] a) {
    return new MyArrayList<T>(a);
}

private static class MyArrayList<T> extends AbstractList<T> {

    private final T[] a;

    MyArrayList(T[] array) {
        a = array;
    }

    public T get(int index) {
        return a[index];
    }

    public T set(int index, T element) {
        T oldValue = a[index];
        a[index] = element;
        return oldValue;
    }

    public int size() {
        return a.length;
    }
}

```

信不信由你，这与包含在`java.util.Arrays`中的实现非常接近。就是这么简单！您提供一个构造函数和`get`，`set`和`size`方法，`AbstractList`会处理其余的所有事情。您将获得`ListIterator`，批量操作，搜索操作，哈希码计算，比较和字符串表示。

假设您想让实现变得更快一点。抽象实现的 API 文档准确描述了每个方法的实现方式，因此您将知道要重写哪些方法以获得所需的性能。前面的实现性能很好，但可以稍微改进一下。特别是，`toArray`方法会遍历`List`，一次复制一个元素。鉴于内部表示，仅克隆数组会更快更合理。

```java
public Object[] toArray() {
    return (Object[]) a.clone();
}

```

添加此覆盖和其他几个类似的覆盖后，此实现与`java.util.Arrays`中找到的实现完全相同。为了充分披露，使用其他抽象实现会有点困难，因为您将不得不编写自己的迭代器，但仍然不是那么困难。

以下列表总结了抽象实现：

+   [`AbstractCollection`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html)  既不是`Set`也不是`List`的`Collection`。至少，您必须提供`iterator`和`size`方法。

+   [`AbstractSet`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html)  一个`Set`；使用方式与`AbstractCollection`相同。

+   [`AbstractList`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractList.html)  由随机访问数据存储支持的`List`。至少，您必须提供`位置访问`方法（`get`，可选的`set`，`remove`和`add`）以及`size`方法。抽象类负责`listIterator`（和`iterator`）。

+   [`AbstractSequentialList`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSequentialList.html)  由顺序访问数据存储支持的`List`，例如链表。至少，你必须提供`listIterator`和`size`方法。抽象类负责处理位置访问方法。（这与`AbstractList`相反。）

+   [`AbstractQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractQueue.html)  至少，你必须提供`offer`、`peek`、`poll`和`size`方法以及支持`remove`的`iterator`。

+   [`AbstractMap`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractMap.html)  一个`Map`。至少你必须提供`entrySet`视图。通常使用`AbstractSet`类来实现。如果`Map`是可修改的，你还必须提供`put`方法。

编写自定义实现的过程如下：

1.  从上述列表中选择适当的抽象实现类。

1.  为类的所有抽象方法提供实现。如果你的自定义集合是可修改的，你还必须重写一个或多个具体方法。抽象实现类的 API 文档将告诉你哪些方法需要重写。

1.  测试并且，如果需要，调试实现。现在你有一个可工作的自定义集合实现。

1.  如果你关心性能，阅读抽象实现类的 API 文档，了解你要继承的所有方法的实现。如果有任何方法看起来太慢，就重写它们。如果你重写任何方法，请确保在重写之前和之后测量方法的性能。调整性能的努力程度应该取决于实现的使用程度以及其使用对性能的关键性。 （通常最好省略此步骤。）
