# 无界通配符

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/unboundedWildcards.html`](https://docs.oracle.com/javase/tutorial/java/generics/unboundedWildcards.html)

未限定通配符类型是使用通配符字符（`?`）指定的，例如，`List<?>`。这被称为*未知类型的列表*。有两种情况下未限定通配符是一个有用的方法：

+   如果你正在编写一个可以使用`Object`类提供的功能来实现的方法。

+   当代码使用泛型类中不依赖于类型参数的方法时。例如，`List.size`或`List.clear`。事实上，`Class<?>`经常被使用，因为`Class<T>`中的大多数方法不依赖于`T`。

考虑以下方法，`printList`：

```java
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}

```

`printList`的目标是打印任何类型的列表，但它未能实现这个目标 — 它只打印`Object`实例的列表；它无法打印`List<Integer>`、`List<String>`、`List<Double>`等，因为它们不是`List<Object>`的子类型。要编写一个通用的`printList`方法，使用`List<?>`：

```java
public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}

```

因为对于任何具体类型`A`，`List<A>`是`List<?>`的子类型，所以你可以使用`printList`来打印任何类型的列表：

```java
List<Integer> li = Arrays.asList(1, 2, 3);
List<String>  ls = Arrays.asList("one", "two", "three");
printList(li);
printList(ls);

```

* * *

**注意：** 在本课程的示例中使用了[`Arrays.asList`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList-T...-)方法。这个静态工厂方法将指定的数组转换并返回一个固定大小的列表。

* * *

需要注意的是，`List<Object>`和`List<?>`并不相同。你可以将`Object`或`Object`的任何子类型插入`List<Object>`中。但你只能将`null`插入`List<?>`中。通配符使用指南部分有关于如何确定在特定情况下应该使用什么类型的通配符的更多信息。
