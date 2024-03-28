# 通配符更有趣

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/morefun.html`](https://docs.oracle.com/javase/tutorial/extra/generics/morefun.html)

在本节中，我们将考虑通配符的一些更高级用法。我们已经看到了几个示例，在从数据结构中读取时有界通配符是有用的。现在考虑相反的情况，一个只写数据结构。接口`Sink`是这种类型的一个简单示例。

```java
interface Sink<T> {
    flush(T t);
}

```

我们可以想象使用它，如下面的代码所示。方法`writeAll()`旨在将集合`coll`的所有元素刷新到接收器`snk`中，并返回最后一个刷新的元素。

```java
public static <T> T writeAll(Collection<T> coll, Sink<T> snk) {
    T last;
    for (T t : coll) {
        last = t;
        snk.flush(last);
    }
    return last;
}
...
Sink<Object> s;
Collection<String> cs;
String str = writeAll(cs, s); // *Illegal call.*

```

如所写，对`writeAll()`的调用是非法的，因为无法推断出有效的类型参数；`String`和`Object`都不适合`T`的类型，因为`Collection`元素和`Sink`元素必须是相同类型。

我们可以通过修改`writeAll()`的签名来修复此错误，如下所示，使用通配符。

```java
public static <T> T writeAll(Collection<? extends T>, Sink<T>) {...}
...
// *Call is OK, but wrong return type.* 
String str = writeAll(cs, s);

```

现在调用是合法的，但赋值是错误的，因为推断的返回类型是`Object`，因为`T`与`s`的元素类型匹配，而`s`是`Object`。

解决方案是使用我们尚未看到的一种有界通配符形式：带有*下界*的通配符。语法`? **super** T`表示一个未知类型，它是`T`的超类型（或`T`本身；请记住超类型关系是自反的）。这是我们一直在使用的有界通配符的对偶，我们使用`? **extends** T`来表示一个未知类型，它是`T`的子类型。

```java
public static <T> T writeAll(Collection<T> coll, Sink<? super T> snk) {
    ...
}
String str = writeAll(cs, s); // *Yes!* 

```

使用这种语法，调用是合法的，并且推断的类型是`String`，如所需。

现在让我们转向一个更现实的例子。`java.util.TreeSet<E>`表示一个按顺序排列的类型为`E`的元素树。构造`TreeSet`的一种方法是将`Comparator`对象传递给构造函数。该比较器将用于根据所需的顺序对`TreeSet`的元素进行排序。

```java
TreeSet(Comparator<E> c) 

```

`Comparator`接口本质上是：

```java
interface Comparator<T> {
    int compare(T fst, T snd);
}

```

假设我们想创建一个`TreeSet<String>`并传入一个合适的比较器，我们需要传递一个可以比较`String`的`Comparator`。这可以通过`Comparator<String>`来完成，但`Comparator<Object>`同样有效。但是，我们将无法在`Comparator<Object>`上调用上面给出的构造函数。我们可以使用下界通配符来获得所需的灵活性：

```java
TreeSet(Comparator<? super E> c) 

```

此代码允许使用任何适用的比较器。

作为使用下界通配符的最后一个示例，让我们看看方法`Collections.max()`，它返回作为参数传递给它的集合中的最大元素。现在，为了让`max()`起作用，传入的集合的所有元素都必须实现`Comparable`。此外，它们都必须可以相互比较。

对这种方法签名的泛型化的第一次尝试产生了：

```java
public static <T extends Comparable<T>> T max(Collection<T> coll)

```

换句话说，该方法接受一个可与自身比较的某种类型`T`的集合，并返回该类型的一个元素。然而，这段代码实际上过于限制性。要了解原因，考虑一种可与任意对象进行比较的类型：

```java
class Foo implements Comparable<Object> {
    ...
}
Collection<Foo> cf = ... ;
Collections.max(cf); // *Should work.*

```

`cf`的每个元素都可以与`cf`中的其他元素进行比较，因为每个这样的元素都是`Foo`，而`Foo`可以与任何对象进行比较，特别是与另一个`Foo`进行比较。然而，使用上面的签名，我们发现该调用被拒绝了。推断的类型必须是`Foo`，但`Foo`并没有实现`Comparable<Foo>`。

`T`与**自身**比较并不是必需的。所需的是`T`与其超类型之一进行比较。这给了我们：

```java
public static <T extends Comparable<? super T>> 
        T max(Collection<T> coll)

```

请注意，`Collections.max()`的实际签名更为复杂。我们将在下一节将遗留代码转换为使用泛型中回到这一点。这种推理几乎适用于任何旨在适用于任意类型的`Comparable`的用法：您总是希望使用`Comparable<? **super** T>`。

一般来说，如果您的 API 只使用类型参数`T`作为参数，那么它的使用应该利用下界通配符（`? **super** T`）。相反，如果 API 只返回`T`，那么使用上界通配符（`? **extends** T`）将为客户端提供更大的灵活性。

## 通配符捕获

现在应该很清楚了，鉴于：

```java
Set<?> unknownSet = new HashSet<String>();
...
/* Add an element  t to a Set s. */ 
public static <T> void addToSet(Set<T> s, T t) {
    ...
}

```

下面的调用是非法的。

```java
addToSet(unknownSet, "abc"); // *Illegal.*

```

实际传递的集合是字符串集合并不重要；重要的是作为参数传递的表达式是未知类型的集合，不能保证是字符串集合，或者是特定类型的集合。

现在，考虑以下代码：

```java
class Collections {
    ...
    <T> public static Set<T> unmodifiableSet(Set<T> set) {
        ...
    }
}
...
Set<?> s = Collections.unmodifiableSet(unknownSet); // *This works! Why?*

```

看起来这不应该被允许；然而，看着这个具体的调用，允许它是完全安全的。毕竟，`unmodifiableSet()`对于任何类型的`Set`都有效，无论其元素类型是什么。

由于这种情况相对频繁出现，有一条特殊规则允许在非常具体的情况下使用这样的代码，其中可以证明代码是安全的。这条规则称为*通配符捕获*，允许编译器将通配符的未知类型推断为泛型方法的类型参数。
