# 泛型方法和有界类型参数

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/boundedTypeParams.html`](https://docs.oracle.com/javase/tutorial/java/generics/boundedTypeParams.html)

有界类型参数是实现泛型算法的关键。考虑以下方法，该方法计算数组`T[]`中大于指定元素`elem`的元素数量。

```java
public static <T> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e > elem)  // compiler error
            ++count;
    return count;
}

```

该方法的实现很简单，但它无法编译，因为大于运算符（`>`）仅适用于原始类型，如`short`、`int`、`double`、`long`、`float`、`byte`和`char`。你不能使用`>`运算符来比较对象。为了解决这个问题，使用一个由`Comparable<T>`接口限定的类型参数：

```java
public interface Comparable<T> {
    public int compareTo(T o);
}

```

最终的代码将是：

```java
public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e.compareTo(elem) > 0)
            ++count;
    return count;
}

```
