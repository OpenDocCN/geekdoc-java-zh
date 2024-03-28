# 擦除通用方法

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/genMethods.html`](https://docs.oracle.com/javase/tutorial/java/generics/genMethods.html)

Java 编译器还会擦除通用方法参数中的类型参数。考虑以下通用方法：

```java
// Counts the number of occurrences of elem in anArray.
//
public static <T> int count(T[] anArray, T elem) {
    int cnt = 0;
    for (T e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}

```

因为`T`是无界的，Java 编译器将其替换为`Object`：

```java
public static int count(Object[] anArray, Object elem) {
    int cnt = 0;
    for (Object e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}

```

假设以下类已被定义：

```java
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }

```

您可以编写一个通用方法来绘制不同的形状：

```java
public static <T extends Shape> void draw(T shape) { /* ... */ }

```

Java 编译器将`T`替换为`Shape`：

```java
public static void draw(Shape shape) { /* ... */ }

```
