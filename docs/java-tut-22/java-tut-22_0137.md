# 问题和练习：泛型

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/QandE/generics-questions.html`](https://docs.oracle.com/javase/tutorial/java/generics/QandE/generics-questions.html)

1.  编写一个通用方法来计算集合中具有特定属性的元素数量（例如，奇数、质数、回文数）。

1.  以下类会编译吗？如果不会，为什么？

    ```java
    public final class Algorithm {
        public static <T> T max(T x, T y) {
            return x > y ? x : y;
        }
    }

    ```

1.  编写一个通用方法来交换数组中两个不同元素的位置。

1.  如果编译器在编译时擦除所有类型参数，为什么应该使用泛型？

1.  在类型擦除后，以下类被转换为什么？

    ```java
    public class Pair<K, V> {

        public Pair(K key, V value) {
            this.key = key;
            this.value = value;
        }

        public K getKey() { return key; }
        public V getValue() { return value; }

        public void setKey(K key)     { this.key = key; }
        public void setValue(V value) { this.value = value; }

        private K key;
        private V value;
    }

    ```

1.  以下方法在类型擦除后会转换为什么？

    ```java
    public static <T extends Comparable<T>>
        int findFirstGreaterThan(T[] at, T elem) {
        // ...
    }

    ```

1.  以下方法会编译吗？如果不会，为什么？

    ```java
    public static void print(List<? extends Number> list) {
        for (Number n : list)
            System.out.print(n + " ");
        System.out.println();
    }

    ```

1.  编写一个通用方法来查找列表范围`begin, end)`中的最大元素。

1.  以下类会编译吗？如果不会，为什么？

    ```java
    public class Singleton<T> {

        public static T getInstance() {
            if (instance == null)
                instance = new Singleton<T>();

            return instance;
        }

        private static T instance = null;
    }

    ```

1.  给定以下类：

    ```java
    class Shape { /* ... */ }
    class Circle extends Shape { /* ... */ }
    class Rectangle extends Shape { /* ... */ }

    class Node<T> { /* ... */ }

    ```

    以下代码会编译吗？如果不会，为什么？

    ```java
    Node<Circle> nc = new Node<>();
    Node<Shape>  ns = nc;

    ```

1.  考虑这个类：

    ```java
    class Node<T> implements Comparable<T> {
        public int compareTo(T obj) { /* ... */ }
        // ...
    }

    ```

    以下代码会编译吗？如果不会，为什么？

    ```java
    Node<String> node = new Node<>();
    Comparable<String> comp = node;

    ```

1.  如何调用以下方法来找到列表中与指定整数列表互质的第一个整数？

    ```java
    public static <T>
        int findFirst(List<T> list, int begin, int end, UnaryPredicate<T> p)

    ```

    注意，两个整数*a*和*b*互质，如果 gcd(*a, b*) = 1，其中 gcd 是最大公约数的缩写。

[检查你的答案。
