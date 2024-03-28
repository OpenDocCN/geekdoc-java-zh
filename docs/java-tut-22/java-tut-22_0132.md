# 泛型类型的擦除

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/genTypes.html`](https://docs.oracle.com/javase/tutorial/java/generics/genTypes.html)

在类型擦除过程中，Java 编译器擦除所有类型参数，并用其第一个边界替换每个类型参数（如果类型参数是有界的），或者用`Object`替换（如果类型参数是无界的）。

考虑下面表示单链表中节点的泛型类：

```java
public class Node<T> {

    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}

```

因为类型参数`T`是无界的，Java 编译器将其替换为`Object`：

```java
public class Node {

    private Object data;
    private Node next;

    public Node(Object data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Object getData() { return data; }
    // ...
}

```

在下面的示例中，泛型`Node`类使用了有界类型参数：

```java
public class Node<T extends Comparable<T>> {

    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}

```

Java 编译器将有界类型参数`T`替换为第一个边界类`Comparable`：

```java
public class Node {

    private Comparable data;
    private Node next;

    public Node(Comparable data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Comparable getData() { return data; }
    // ...
}

```
