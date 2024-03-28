# 缩减

> 原文：[`docs.oracle.com/javase/tutorial/collections/streams/reduction.html`](https://docs.oracle.com/javase/tutorial/collections/streams/reduction.html)

章节聚合操作描述了以下操作流水线，它计算`roster`集合中所有男性成员的平均年龄：

```java
double average = roster
    .stream()
    .filter(p -> p.getGender() == Person.Sex.MALE)
    .mapToInt(Person::getAge)
    .average()
    .getAsDouble();
```

JDK 包含许多终端操作（如[`average`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#average--java/lang/reflect/Executable.html)、[`sum`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#sum--)、[`min`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#min-java.util.Comparator-)、[`max`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#max-java.util.Comparator-) 和[`count`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#count--)），它们通过组合流的内容返回一个值。这些操作称为*缩减操作*。JDK 还包含返回集合而不是单个值的缩减操作。许多缩减操作执行特定任务，比如找到值的平均值或将元素分组到类别中。然而，JDK 为您提供了通用的缩减操作[`reduce`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-T-java.util.function.BinaryOperator-) 和[`collect`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BiConsumer-)，本节将详细描述这些操作。

本节涵盖以下主题：

+   Stream.reduce 方法

+   Stream.collect 方法

你可以在示例`ReductionExamples`中找到本节中描述的代码片段。

## `Stream.reduce` 方法

[`Stream.reduce`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-T-java.util.function.BinaryOperator-) 方法是一个通用的缩减操作。考虑以下流水线，它计算`roster`集合中男性成员年龄的总和。它使用[`Stream.sum`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#sum--) 缩减操作：

```java
Integer totalAge = roster
    .stream()
    .mapToInt(Person::getAge)
    .sum();
```

将此与以下使用`Stream.reduce`操作计算相同值的流水线进行比较：

```java
Integer totalAgeReduce = roster
   .stream()
   .map(Person::getAge)
   .reduce(
       0,
       (a, b) -> a + b);
```

在这个例子中，`reduce` 操作接受两个参数：

+   `identity`：身份元素既是缩减的初始值，也是如果流中没有元素时的默认结果。在这个例子中，身份元素是`0`；这是年龄总和的初始值，也是如果`roster`集合中不存在成员时的默认值。

+   `accumulator`: 累加器函数接受两个参数：减少的部分结果（在这个例子中，到目前为止所有处理过的整数的总和）和流的下一个元素（在这个例子中，一个整数）。它返回一个新的部分结果。在这个例子中，累加器函数是一个 lambda 表达式，它将两个`Integer`值相加并返回一个`Integer`值：

    ```java
    (a, b) -> a + b
    ```

`reduce`操作总是返回一个新值。然而，累加器函数在处理流的每个元素时也返回一个新值。假设你想将流的元素减少到一个更复杂的对象，比如一个集合。这可能会影响你的应用程序的性能。如果你的`reduce`操作涉及将元素添加到一个集合中，那么每次累加器函数处理一个元素时，它都会创建一个包含该元素的新集合，这是低效的。更新现有集合会更有效。你可以使用[`Stream.collect`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BiConsumer-)方法来实现这一点，下一节将介绍。

## `collect` 方法

与`reduce`方法不同，它在处理元素时总是创建一个新值，[`collect`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BiConsumer-)方法修改或改变了现有值。

考虑如何在流中找到值的平均值。你需要两个数据：值的总数和这些值的总和。然而，像`reduce`方法和所有其他减少方法一样，`collect`方法只返回一个值。你可以创建一个包含成员变量来跟踪值的总数和这些值总和的新数据类型，比如下面的类`Averager`：

```java
class Averager implements IntConsumer
{
    private int total = 0;
    private int count = 0;

    public double average() {
        return count > 0 ? ((double) total)/count : 0;
    }

    public void accept(int i) { total += i; count++; }
    public void combine(Averager other) {
        total += other.total;
        count += other.count;
    }
}
```

以下管道使用`Averager`类和`collect`方法来计算所有男性成员的平均年龄：

```java
Averager averageCollect = roster.stream()
    .filter(p -> p.getGender() == Person.Sex.MALE)
    .map(Person::getAge)
    .collect(Averager::new, Averager::accept, Averager::combine);

System.out.println("Average age of male members: " +
    averageCollect.average());
```

这个例子中的`collect`操作接受三个参数：

+   `supplier`: 供应商是一个工厂函数；它构造新实例。对于`collect`操作，它创建结果容器的实例。在这个例子中，它是`Averager`类的一个新实例。

+   `accumulator`: 累加器函数将流元素合并到结果容器中。在这个例子中，它通过将`count`变量加一并将流元素的值（代表男性成员年龄的整数）加到`total`成员变量中，修改了`Averager`结果容器。

+   `combiner`：合并器函数接受两个结果容器并合并它们的内容。在这个例子中，它通过增加另一个`Averager`实例的`count`成员变量的值到`Averager`结果容器的`count`变量，并将另一个`Averager`实例的`total`成员变量的值加到`total`成员变量中来修改`Averager`结果容器。

请注意以下内容：

+   供应商是一个 lambda 表达式（或方法引用），与`reduce`操作中的单位元素等值相对。

+   累加器和合并器函数不返回值。

+   您可以在并行流中使用`collect`操作；有关更多信息，请参阅并行性部分。（如果您在并行流中运行`collect`方法，那么当合并器函数创建一个新对象时，例如在这个例子中创建一个`Averager`对象时，JDK 会创建一个新线程。因此，您不必担心同步。）

尽管 JDK 为您提供了`average`操作来计算流中元素的平均值，但如果您需要从流的元素计算多个值，可以使用`collect`操作和自定义类。

`collect`操作最适合集合。以下示例使用`collect`操作将男性成员的姓名放入集合中：

```java
List<String> namesOfMaleMembersCollect = roster
    .stream()
    .filter(p -> p.getGender() == Person.Sex.MALE)
    .map(p -> p.getName())
    .collect(Collectors.toList());
```

此版本的`collect`操作接受一个类型为[`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html)的参数。这个类封装了在`collect`操作中用作参数的函数，该操作需要三个参数（供应商、累加器和合并器函数）。

[`Collectors`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)类包含许多有用的归约操作，例如将元素累积到集合中并根据各种标准对元素进行总结。这些归约操作返回`Collector`类的实例，因此您可以将它们作为`collect`操作的参数使用。

此示例使用[`Collectors.toList`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toList--)操作，将流元素累积到一个新的`List`实例中。与`Collectors`类中的大多数操作一样，`toList`操作符返回一个`Collector`实例，而不是一个集合。

以下示例按性别对集合`roster`的成员进行分组：

```java
Map<Person.Sex, List<Person>> byGender =
    roster
        .stream()
        .collect(
            Collectors.groupingBy(Person::getGender));
```

[`groupingBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#groupingBy-java.util.function.Function-) 操作返回一个映射，其键是应用作为其参数指定的 lambda 表达式（称为*分类函数*）的结果值。在这个例子中，返回的映射包含两个键，`Person.Sex.MALE` 和 `Person.Sex.FEMALE`。键对应的值是包含通过分类函数处理时对应于键值的流元素的 `List` 实例。例如，对应于键 `Person.Sex.MALE` 的值是一个包含所有男性成员的 `List` 实例。

以下示例按性别检索集合 `roster` 中每个成员的名称并按性别分组：

```java
Map<Person.Sex, List<String>> namesByGender =
    roster
        .stream()
        .collect(
            Collectors.groupingBy(
                Person::getGender,                      
                Collectors.mapping(
                    Person::getName,
                    Collectors.toList())));
```

在这个例子中，[`groupingBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#groupingBy-java.util.function.Function-java.util.stream.Collector-) 操作需要两个参数，一个分类函数和一个 `Collector` 实例。`Collector` 参数称为*下游收集器*。这是 Java 运行时应用于另一个收集器结果的收集器。因此，这个 `groupingBy` 操作使您能够对 `groupingBy` 操作符创建的 `List` 值应用 `collect` 方法。这个例子应用了收集器 [`mapping`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#mapping-java.util.function.Function-java.util.stream.Collector-java.util.stream.Collector-)，它将映射函数 `Person::getName` 应用于流的每个元素。因此，结果流只包含成员的名称。包含一个或多个下游收集器的管道，像这个例子一样，称为*多级减少*。

以下示例检索每个性别成员的总年龄：

```java
Map<Person.Sex, Integer> totalAgeByGender =
    roster
        .stream()
        .collect(
            Collectors.groupingBy(
                Person::getGender,                      
                Collectors.reducing(
                    0,
                    Person::getAge,
                    Integer::sum)));
```

[`reducing`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#reducing-U-java.util.function.Function-java.util.function.BinaryOperator-) 操作需要三个参数：

+   `identity`：类似于 `Stream.reduce` 操作，身份元素既是减少的初始值，也是如果流中没有元素时的默认结果。在这个例子中，身份元素是 `0`；这是年龄总和的初始值，如果没有成员存在，则是默认值。

+   `mapper`：`reducing` 操作将此映射函数应用于所有流元素。在这个例子中，映射器检索每个成员的年龄。

+   `operation`：操作函数用于减少映射值。在这个例子中，操作函数添加 `Integer` 值。

以下示例检索每个性别成员的平均年龄：

```java
Map<Person.Sex, Double> averageAgeByGender = roster
    .stream()
    .collect(
        Collectors.groupingBy(
            Person::getGender,                      
            Collectors.averagingInt(Person::getAge)));
```
