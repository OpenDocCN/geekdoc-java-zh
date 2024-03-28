# 并行性

> 原文：[`docs.oracle.com/javase/tutorial/collections/streams/parallelism.html`](https://docs.oracle.com/javase/tutorial/collections/streams/parallelism.html)

并行计算涉及将问题分解为子问题，同时解决这些问题（并行执行，每个子问题在单独的线程中运行），然后将这些子问题的解决方案合并。Java SE 提供了分支/合并框架，它使您能够更轻松地在应用程序中实现并行计算。然而，使用此框架时，您必须指定如何将问题细分（分区）。使用聚合操作，Java 运行时为您执行此分区和解决方案的合并。

在使用集合的应用程序中实现并行性的一个困难在于集合不是线程安全的，这意味着多个线程不能在不引入线程干扰或内存一致性错误的情况下操作集合。集合框架提供了同步包装器，它可以为任意集合添加自动同步，使其线程安全。然而，同步会引入线程争用。你应该避免线程争用，因为它会阻止线程并行运行。聚合操作和并行流使你能够在非线程安全的集合上实现并行性，前提是在操作集合时不修改它。

请注意，并行性并不自动比串行执行操作更快，尽管在有足够数据和处理器核心的情况下可能会更快。虽然聚合操作使您更容易实现并行性，但确定您的应用程序是否适合并行性仍然是您的责任。

本节涵盖以下主题：

+   并行执行流

+   并发归约

+   顺序

+   副作用

    +   懒惰性

    +   干扰

    +   有状态的 Lambda 表达式

你可以在示例`ParallelismExamples`中找到本节中描述的代码摘录。

## 并行执行流

你可以串行或并行执行流。当流并行执行时，Java 运行时将流分成多个子流。聚合操作并行迭代和处理这些子流，然后将结果合并。

当您创建一个流时，除非另有说明，它总是一个串行流。要创建一个并行流，请调用操作[`Collection.parallelStream`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#parallelStream--)。或者，调用操作[`BaseStream.parallel`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#parallel--)。例如，以下语句计算所有男性成员的平均年龄：

```java
double average = roster
    .parallelStream()
    .filter(p -> p.getGender() == Person.Sex.MALE)
    .mapToInt(Person::getAge)
    .average()
    .getAsDouble();
```

## 并发减少

再次考虑以下示例（在减少部分中描述），该示例按性别对成员进行分组。此示例调用`collect`操作，将集合`roster`减少为`Map`：

```java
Map<Person.Sex, List<Person>> byGender =
    roster
        .stream()
        .collect(
            Collectors.groupingBy(Person::getGender));
```

以下是并行等价的：

```java
ConcurrentMap<Person.Sex, List<Person>> byGender =
    roster
        .parallelStream()
        .collect(
            Collectors.groupingByConcurrent(Person::getGender));
```

这被称为*并发减少*。如果包含`collect`操作的特定管道满足以下所有条件，则 Java 运行时执行并发减少：

+   流是并行的。

+   `collect`操作的参数，收集器，具有特征[`Collector.Characteristics.CONCURRENT`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.Characteristics.html#CONCURRENT)。要确定收集器的特征，请调用[`Collector.characteristics`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.Characteristics.html)方法。

+   流要么是无序的，要么收集器具有特征[`Collector.Characteristics.UNORDERED`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.Characteristics.html#UNORDERED)。要确保流是无序的，请调用[`BaseStream.unordered`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#unordered--)操作。

**注意**：此示例返回一个[`ConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html)的实例，而不是`Map`，并调用[`groupingByConcurrent`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#groupingByConcurrent-java.util.function.Function-)操作，而不是`groupingBy`。（有关`ConcurrentMap`的更多信息，请参见并发集合部分。）与操作`groupingByConcurrent`不同，操作`groupingBy`在并行流中表现不佳。（这是因为它通过键合并两个映射，这在计算上是昂贵的。）同样，操作[`Collectors.toConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toConcurrentMap-java.util.function.Function-java.util.function.Function-)在并行流中的性能优于操作[`Collectors.toMap`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toMap-java.util.function.Function-java.util.function.Function-)。

## 排序

流水线处理流的元素的顺序取决于流是串行执行还是并行执行、流的来源以及中间操作。例如，考虑以下示例，该示例多次使用`forEach`操作打印`ArrayList`实例的元素：

```java
Integer[] intArray = {1, 2, 3, 4, 5, 6, 7, 8 };
List<Integer> listOfIntegers =
    new ArrayList<>(Arrays.asList(intArray));

System.out.println("listOfIntegers:");
listOfIntegers
    .stream()
    .forEach(e -> System.out.print(e + " "));
System.out.println("");

System.out.println("listOfIntegers sorted in reverse order:");
Comparator<Integer> normal = Integer::compare;
Comparator<Integer> reversed = normal.reversed(); 
Collections.sort(listOfIntegers, reversed);  
listOfIntegers
    .stream()
    .forEach(e -> System.out.print(e + " "));
System.out.println("");

System.out.println("Parallel stream");
listOfIntegers
    .parallelStream()
    .forEach(e -> System.out.print(e + " "));
System.out.println("");

System.out.println("Another parallel stream:");
listOfIntegers
    .parallelStream()
    .forEach(e -> System.out.print(e + " "));
System.out.println("");

System.out.println("With forEachOrdered:");
listOfIntegers
    .parallelStream()
    .forEachOrdered(e -> System.out.print(e + " "));
System.out.println("");
```

该示例包含五个流水线。它打印类似以下的输出：

```java
listOfIntegers:
1 2 3 4 5 6 7 8
listOfIntegers sorted in reverse order:
8 7 6 5 4 3 2 1
Parallel stream:
3 4 1 6 2 5 7 8
Another parallel stream:
6 3 1 5 7 8 4 2
With forEachOrdered:
8 7 6 5 4 3 2 1
```

该示例执行以下操作：

+   第一个流水线按照它们添加到列表中的顺序打印`listOfIntegers`列表的元素。

+   第二个流水线在使用[`Collections.sort`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List-)方法对`listOfIntegers`进行排序后打印元素。

+   第三和第四个流水线以一种看似随机的顺序打印列表的元素。请记住，流操作在处理流的元素时使用内部迭代。因此，当您并行执行流时，除非流操作另有规定，否则 Java 编译器和运行时会确定处理流元素的顺序，以最大化并行计算的好处。

+   第五个流水线使用[`forEachOrdered`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#forEachOrdered-java.util.function.Consumer-)方法，该方法按照其来源指定的顺序处理流的元素，无论您是串行还是并行执行流。请注意，如果您在并行流中使用类似`forEachOrdered`的操作，可能会丧失并行性的好处。

## 副作用

如果一个方法或表达式除了返回或产生一个值外，还修改了计算机的状态，那么它就具有副作用。例如，可变归约（使用`collect`操作的操作；有关更多信息，请参见 Reduction 部分）以及调用`System.out.println`方法进行调试。JDK 在流水线中处理某些副作用很好。特别是，`collect`方法被设计为以并行安全的方式执行具有副作用的最常见流操作。`forEach`和`peek`等操作是为了副作用而设计的；一个返回 void 的 Lambda 表达式，比如调用`System.out.println`的 Lambda 表达式，除了具有副作用外什么也做不了。即便如此，你应该谨慎使用`forEach`和`peek`操作；如果你在并行流中使用其中一个操作，那么 Java 运行时可能会从多个线程同时调用你指定为参数的 Lambda 表达式。此外，永远不要在`filter`和`map`等操作中传递具有副作用的 Lambda 表达式作为参数。接下来的章节讨论干扰和有状态的 Lambda 表达式，它们都可能是副作用的来源，并且可能返回不一致或不可预测的结果，特别是在并行流中。然而，首先讨论懒惰的概念，因为它对干扰有直接影响。

### 懒惰

所有中间操作都是*懒惰*的。如果一个表达式、方法或算法只有在需要时才会被评估，那么它就是懒惰的（如果算法立即被评估或处理，则是*急切*的）。中间操作是懒惰的，因为它们直到终端操作开始才开始处理流的内容。懒惰地处理流使得 Java 编译器和运行时能够优化它们处理流的方式。例如，在像`filter`-`mapToInt`-`average`这样的流水线中，`average`操作可以从`mapToInt`操作创建的流中获取前几个整数，而这些整数是从`filter`操作获取的。`average`操作会重复这个过程，直到它从流中获取了所有需要的元素，然后计算平均值。

### 干扰

流操作中的 Lambda 表达式不应该*干扰*。当流的源在流水线处理流时被修改时就会发生干扰。例如，下面的代码尝试连接`List` `listOfStrings` 中包含的字符串。然而，它会抛出`ConcurrentModificationException`：

```java
try {
    List<String> listOfStrings =
        new ArrayList<>(Arrays.asList("one", "two"));

    // This will fail as the peek operation will attempt to add the
    // string "three" to the source after the terminal operation has
    // commenced. 

    String concatenatedString = listOfStrings
        .stream()

        // Don't do this! Interference occurs here.
        .peek(s -> listOfStrings.add("three"))

        .reduce((a, b) -> a + " " + b)
        .get();

    System.out.println("Concatenated string: " + concatenatedString);

} catch (Exception e) {
    System.out.println("Exception caught: " + e.toString());
}
```

此示例使用`reduce`操作将`listOfStrings` 中包含的字符串连接成一个`Optional<String>` 值，`reduce`是一个终端操作。然而，此处的管道调用了中间操作`peek`，它试图向`listOfStrings`添加一个新元素。请记住，所有中间操作都是惰性的。这意味着在此示例中，管道在调用操作`get`时开始执行，并在`get`操作完成时结束执行。`peek`操作的参数在管道执行过程中尝试修改流源，这会导致 Java 运行时抛出`ConcurrentModificationException`。

### 有状态的 Lambda 表达式

避免在流操作中将*有状态的 lambda 表达式*用作参数。有状态的 lambda 表达式是指其结果取决于在管道执行过程中可能发生变化的任何状态。以下示例使用`map`中间操作将`List` `listOfIntegers` 中的元素添加到新的`List`实例中。它分别使用串行流和并行流执行两次：

```java
List<Integer> serialStorage = new ArrayList<>();

System.out.println("Serial stream:");
listOfIntegers
    .stream()

    // Don't do this! It uses a stateful lambda expression.
    .map(e -> { serialStorage.add(e); return e; })

    .forEachOrdered(e -> System.out.print(e + " "));
System.out.println("");

serialStorage
    .stream()
    .forEachOrdered(e -> System.out.print(e + " "));
System.out.println("");

System.out.println("Parallel stream:");
List<Integer> parallelStorage = Collections.synchronizedList(
    new ArrayList<>());
listOfIntegers
    .parallelStream()

    // Don't do this! It uses a stateful lambda expression.
    .map(e -> { parallelStorage.add(e); return e; })

    .forEachOrdered(e -> System.out.print(e + " "));
System.out.println("");

parallelStorage
    .stream()
    .forEachOrdered(e -> System.out.print(e + " "));
System.out.println("");
```

Lambda 表达式`e -> { parallelStorage.add(e); return e; }`是一个有状态的 lambda 表达式。其结果可能每次运行代码时都会有所不同。此示例打印如下内容：

```java
Serial stream:
8 7 6 5 4 3 2 1
8 7 6 5 4 3 2 1
Parallel stream:
8 7 6 5 4 3 2 1
1 3 6 2 4 5 8 7
```

操作`forEachOrdered`按照流指定的顺序处理元素，无论流是串行还是并行执行。然而，当流并行执行时，`map`操作处理由 Java 运行时和编译器指定的流元素。因此，lambda 表达式`e -> { parallelStorage.add(e); return e; }`向`List` `parallelStorage`添加元素的顺序可能每次运行代码时都会有所不同。为了获得确定性和可预测的结果，请确保流操作中的 lambda 表达式参数不是有状态的。

**注意**：此示例调用了方法[`synchronizedList`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#synchronizedList-java.util.List-)，以使`List` `parallelStorage` 线程安全。请记住，集合不是线程安全的。这意味着多个线程不应同时访问特定集合。假设在创建`parallelStorage`时未调用方法`synchronizedList`：

```java
List<Integer> parallelStorage = new ArrayList<>();
```

该示例行为不稳定，因为多个线程访问和修改`parallelStorage`而没有像同步这样的机制来安排特定线程何时可以访问`List`实例。因此，该示例可能打印类似以下内容的输出：

```java
Parallel stream:
8 7 6 5 4 3 2 1
null 3 5 4 7 8 1 2
```
