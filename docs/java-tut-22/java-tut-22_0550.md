# 兼容性

> 原文：[`docs.oracle.com/javase/tutorial/collections/interoperability/compatibility.html`](https://docs.oracle.com/javase/tutorial/collections/interoperability/compatibility.html)

Java 集合框架旨在确保核心集合接口与早期 Java 平台版本中用于表示集合的类型之间的完全互操作性：[`Vector`](https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html)，[`Hashtable`](https://docs.oracle.com/javase/8/docs/api/java/util/Hashtable.html)，数组，以及[`Enumeration`](https://docs.oracle.com/javase/8/docs/api/java/util/Enumeration.html)。在本节中，您将学习如何将旧集合转换为 Java 集合框架集合，反之亦然。

## 向上兼容性

假设你正在使用一个返回传统集合的 API 以及另一个 API，需要对象实现集合接口。为了使这两个 API 顺利互操作，你必须将传统集合转换为现代集合。幸运的是，Java 集合框架使这变得容易。

假设旧的 API 返回一个对象数组，而新的 API 需要一个`Collection`。集合框架有一个方便的实现，允许将对象数组视为`List`。你可以使用[`Arrays.asList`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList-T...-)将数组传递给任何需要`Collection`或`List`的方法。

```java
Foo[] result = oldMethod(arg);
newMethod(Arrays.asList(result));

```

如果旧的 API 返回一个`Vector`或`Hashtable`，你根本不需要做任何工作，因为`Vector`已经被改装为实现`List`接口，而`Hashtable`已经被改装为实现`Map`。因此，`Vector`可以直接传递给任何需要`Collection`或`List`的方法。

```java
Vector result = oldMethod(arg);
newMethod(result);

```

类似地，`Hashtable`可以直接传递给任何需要`Map`的方法。

```java
Hashtable result = oldMethod(arg);
newMethod(result);

```

较少情况下，一个 API 可能返回一个代表对象集合的`Enumeration`。`Collections.list`方法将`Enumeration`转换为`Collection`。

```java
Enumeration e = oldMethod(arg);
newMethod(Collections.list(e));

```

## 向后兼容性

假设你正在使用一个返回现代集合的 API 以及另一个 API，需要你传递传统集合。为了使这两个 API 顺利互操作，你必须将现代集合转换为旧集合。同样，Java 集合框架使这变得容易。

假设新的 API 返回一个`Collection`，而旧的 API 需要一个`Object`数组。正如你可能知道的那样，`Collection`接口包含一个专门设计用于这种情况的`toArray`方法。

```java
Collection c = newMethod();
oldMethod(c.toArray());

```

如果旧的 API 需要一个`String`数组（或其他类型）而不是一个`Object`数组怎么办？你只需使用`toArray`的另一种形式  需要一个数组作为输入的形式。

```java
Collection c = newMethod();
oldMethod((String[]) c.toArray(new String[0]));

```

如果旧的 API 需要一个`Vector`，标准集合构造函数会派上用场。

```java
Collection c = newMethod();
oldMethod(new Vector(c));

```

需要`Hashtable`的旧 API 的情况类似处理。

```java
Map m = newMethod();
oldMethod(new Hashtable(m));

```

最后，如果旧的 API 需要一个`Enumeration`怎么办？这种情况并不常见，但偶尔会发生，[`Collections.enumeration`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#enumeration-java.util.Collection-) 方法就是为了处理这种情况而提供的。这是一个静态工厂方法，接受一个`Collection`并返回该`Collection`中元素的`Enumeration`。

```java
Collection c = newMethod();
oldMethod(Collections.enumeration(c));

```
