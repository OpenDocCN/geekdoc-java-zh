# 泛型方法

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/methods.html`](https://docs.oracle.com/javase/tutorial/java/generics/methods.html)

*泛型方法* 是引入自己类型参数的方法。这类似于声明一个泛型类型，但类型参数的范围仅限于声明它的方法。允许静态和非静态泛型方法，以及泛型类构造方法。

泛型方法的语法包括一个类型参数列表，在方法返回类型之前出现在尖括号内。对于静态泛型方法，类型参数部分必须出现在方法返回类型之前。

`Util` 类包含一个泛型方法 `compare`，用于比较两个 `Pair` 对象：

```java
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}

public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}

```

调用这个方法的完整语法如下：

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);

```

类型已经明确提供，如粗体所示。通常情况下，这部分可以省略，编译器会推断所需的类型：

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);

```

这个特性被称为*类型推断*，允许您将泛型方法作为普通方法调用，而无需在尖括号之间指定类型。这个主题在下一节 类型推断 中进一步讨论。
