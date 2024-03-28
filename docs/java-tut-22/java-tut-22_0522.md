# 集合接口

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/collection.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/collection.html)

一个[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)代表一组被称为元素的对象。`Collection`接口用于传递希望具有最大一般性的对象集合。例如，按照惯例，所有通用集合实现都有一个接受`Collection`参数的构造函数。这个构造函数，称为*转换构造函数*，初始化新集合以包含指定集合中的所有元素，无论给定集合的子接口或实现类型如何。换句话说，它允许你*转换*集合的类型。

例如，假设您有一个`Collection<String> c`，它可以是`List`，`Set`或另一种类型的`Collection`。这种习惯用法创建一个新的`ArrayList`（`List`接口的实现），最初包含`c`中的所有元素。

```java
List<String> list = new ArrayList<String>(c);

```

或者 - 如果您使用的是 JDK 7 或更高版本 - 您可以使用菱形操作符：

```java
List<String> list = new ArrayList<>(c);

```

`Collection`接口包含执行基本操作的方法，如`int size()`，`boolean isEmpty()`，`boolean contains(Object element)`，`boolean add(E element)`，`boolean remove(Object element)`和`Iterator<E> iterator()`。

它还包含对整个集合进行操作的方法，如`boolean containsAll(Collection<?> c)`，`boolean addAll(Collection<? extends E> c)`，`boolean removeAll(Collection<?> c)`，`boolean retainAll(Collection<?> c)`和`void clear()`。

还存在用于数组操作的其他方法（如`Object[] toArray()`和`<T> T[] toArray(T[] a)`）。

在 JDK 8 及更高版本中，`Collection`接口还公开了方法`Stream<E> stream()`和`Stream<E> parallelStream()`，用于从底层集合获取顺序或并行流。 （有关使用流的更多信息，请参见名为聚合操作的课程。）

`Collection`接口做了你所期望的事情，因为`Collection`代表一组对象。它有告诉你集合中有多少元素的方法（`size`，`isEmpty`），检查给定对象是否在集合中的方法（`contains`），向集合中添加和删除元素的方法（`add`，`remove`），以及提供集合迭代器的方法（`iterator`）。

`add`方法定义得足够通用，以便对允许重复的集合和不允许重复的集合都有意义。它保证在调用完成后`Collection`将包含指定的元素，并且如果调用导致`Collection`发生更改，则返回`true`。类似地，`remove`方法旨在从`Collection`中删除指定元素的单个实例，假设它起初包含该元素，并在结果中修改`Collection`时返回`true`。

## 遍历集合

有三种遍历集合的方式：(1) 使用聚合操作 (2) 使用`for-each`结构和 (3) 使用`Iterator`。

### 聚合操作

在 JDK 8 及更高版本中，迭代集合的首选方法是获取流并对其执行聚合操作。聚合操作通常与 lambda 表达式结合使用，使编程更具表现力，代码行数更少。以下代码顺序迭代形状集合并打印出红色对象：

```java
myShapesCollection.stream()
.filter(e -> e.getColor() == Color.RED)
.forEach(e -> System.out.println(e.getName()));

```

同样，您可以轻松请求并行流，如果集合足够大且计算机具有足够的核心，则可能是有意义的：

```java
myShapesCollection.parallelStream()
.filter(e -> e.getColor() == Color.RED)
.forEach(e -> System.out.println(e.getName()));

```

使用此 API 收集数据有许多不同的方法。例如，您可能希望将`Collection`的元素转换为`String`对象，然后以逗号分隔它们：

```java
    String joined = elements.stream()
    .map(Object::toString)
    .collect(Collectors.joining(", "));

```

或者计算所有员工的工资总和：

```java
int total = employees.stream()
.collect(Collectors.summingInt(Employee::getSalary)));

```

这些只是使用流和聚合操作可以做的一些示例。有关更多信息和示例，请参阅名为 Aggregate Operations 的课程。

集合框架始终作为其 API 的一部分提供了许多所谓的“批量操作”。这些包括操作整个集合的方法，如`containsAll`、`addAll`、`removeAll`等。不要将这些方法与 JDK 8 中引入的聚合操作混淆。新聚合操作和现有的批量操作（`containsAll`、`addAll`等）之间的关键区别在于旧版本都是*变异的*，意味着它们都修改基础集合。相反，新的聚合操作*不*修改基础集合。在使用新的聚合操作和 lambda 表达式时，您必须小心避免突变，以免在将来从并行流运行代码时引入问题。

### for-each 结构

`for-each`结构允许您简洁地使用`for`循环遍历集合或数组  请参阅 for 语句。以下代码使用`for-each`结构将集合的每个元素打印在单独的行上。

```java
for (Object o : collection)
    System.out.println(o);

```

### 迭代器

一个[`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)是一个对象，它使您能够遍历集合并根据需要选择性地删除集合中的元素。通过调用其`iterator`方法，您可以为集合获取一个`Iterator`。以下是`Iterator`接口。

```java
public interface Iterator<E> {
    boolean hasNext();
    E next();
    void remove(); //optional
}

```

`hasNext`方法在迭代还有更多元素时返回`true`，`next`方法返回迭代中的下一个元素。`remove`方法从底层`Collection`中删除上次由`next`返回的最后一个元素。`remove`方法每次调用`next`只能调用一次，如果违反此规则会抛出异常。

请注意，`Iterator.remove`是在迭代期间修改集合的*唯一*安全方式；如果在迭代进行时以任何其他方式修改底层集合，则行为是未指定的。

当您需要时，请使用`Iterator`而不是`for-each`结构：

+   删除当前元素。`for-each`结构隐藏了迭代器，因此无法调用`remove`。因此，`for-each`结构不适用于过滤。

+   并行迭代多个集合。

以下方法向您展示如何使用`Iterator`来过滤任意的`Collection`  也就是遍历集合并删除特定元素。

```java
static void filter(Collection<?> c) {
    for (Iterator<?> it = c.iterator(); it.hasNext(); )
        if (!cond(it.next()))
            it.remove();
}

```

这段简单的代码是多态的，这意味着它适用于*任何*`Collection`，无论实现方式如何。此示例演示了使用 Java 集合框架编写多态算法有多么容易。

## 集合接口批量操作

*批量操作*在整个`Collection`上执行操作。您可以使用基本操作实现这些简写操作，尽管在大多数情况下，这样的实现效率会更低。以下是批量操作：

+   `containsAll`  如果目标`Collection`包含指定`Collection`中的所有元素，则返回`true`。

+   `addAll`  将指定`Collection`中的所有元素添加到目标`Collection`中。

+   `removeAll`  从目标`Collection`中删除所有也包含在指定`Collection`中的元素。

+   `retainAll`  从目标`Collection`中删除所有不包含在指定`Collection`中的元素。也就是说，它仅保留目标`Collection`中也包含在指定`Collection`中的那些元素。

+   `clear`  从`Collection`中删除所有元素。

`addAll`、`removeAll`和`retainAll`方法在执行操作过程中修改了目标`Collection`时都会返回`true`。

作为批量操作强大性的一个简单示例，考虑以下惯用法来从`Collection`中删除所有指定元素`e`的*所有*实例，`c`。

```java
c.removeAll(Collections.singleton(e));

```

更具体地说，假设您想要从`Collection`中删除所有`null`元素。

```java
c.removeAll(Collections.singleton(null));

```

这种习惯用法使用 `Collections.singleton`，这是一个静态工厂方法，返回一个只包含指定元素的不可变 `Set`。

## 集合接口数组操作

`toArray` 方法作为集合和旧 API 之间的桥梁提供，这些旧 API 期望输入数组。数组操作允许将 `Collection` 的内容转换为数组。没有参数的简单形式创建一个新的 `Object` 数组。更复杂的形式允许调用者提供一个数组或选择输出数组的运行时类型。

例如，假设 `c` 是一个 `Collection`。以下代码段将 `c` 的内容转储到一个新分配的 `Object` 数组中，其长度与 `c` 中的元素数量相同。

```java
Object[] a = c.toArray();

```

假设 `c` 只包含字符串（也许是因为 `c` 的类型是 `Collection<String>`）。以下代码段将 `c` 的内容转储到一个新分配的 `String` 数组中，其长度与 `c` 中的元素数量相同。

```java
String[] a = c.toArray(new String[0]);

```
