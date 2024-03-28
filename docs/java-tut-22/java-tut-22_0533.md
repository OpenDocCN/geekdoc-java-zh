# 课程：聚合操作

> 原文：[`docs.oracle.com/javase/tutorial/collections/streams/index.html`](https://docs.oracle.com/javase/tutorial/collections/streams/index.html)

**注意**：要更好地理解本节中的概念，请查看 Lambda 表达式和方法引用部分。

您使用集合做什么？您不仅仅将对象存储在集合中并将其留在那里。在大多数情况下，您使用集合来检索其中存储的项目。

再次考虑 Lambda 表达式部分中描述的场景。假设您正在创建一个社交网络应用。您希望创建一个功能，使管理员能够对满足某些条件的社交网络应用成员执行任何操作，例如发送消息。

与之前一样，假设这个社交网络应用的成员由以下`Person`类表示：

```java
public class Person {

    public enum Sex {
        MALE, FEMALE
    }

    String name;
    LocalDate birthday;
    Sex gender;
    String emailAddress;

    // ...

    public int getAge() {
        // ...
    }

    public String getName() {
        // ...
    }
}
```

以下示例使用 for-each 循环打印集合`roster`中包含的所有成员的名称：

```java
for (Person p : roster) {
    System.out.println(p.getName());
}
```

以下示例使用聚合操作`forEach`打印出集合`roster`中包含的所有成员：

```java
roster
    .stream()
    .forEach(e -> System.out.println(e.getName());
```

尽管在此示例中，使用聚合操作的版本比使用 for-each 循环的版本更长，但您将看到，对于更复杂的任务，使用批量数据操作的版本将更简洁。

下面涵盖了以下主题：

+   管道和流

+   聚合操作和迭代器之间的区别

在示例`BulkDataOperationsExamples`中找到本节描述的代码片段。

### 管道和流

*管道*是一系列聚合操作。以下示例使用由聚合操作`filter`和`forEach`组成的管道打印集合`roster`中包含的男性成员：

```java
roster
    .stream()
    .filter(e -> e.getGender() == Person.Sex.MALE)
    .forEach(e -> System.out.println(e.getName()));
```

将此示例与使用 for-each 循环打印集合`roster`中的男性成员的示例进行比较：

```java
for (Person p : roster) {
    if (p.getGender() == Person.Sex.MALE) {
        System.out.println(p.getName());
    }
}
```

一个管道包含以下组件：

+   源：这可以是集合、数组、生成器函数或 I/O 通道。在此示例中，源是集合`roster`。

+   零个或多个*中间操作*。中间操作，如`filter`，会生成一个新流。

    *流*是元素的序列。与集合不同，它不是存储元素的数据结构。相反，流通过管道从源头传递值。此示例通过调用`stream`方法从集合`roster`创建流。

    `filter`操作返回一个包含与其谓词（该操作的参数）匹配的元素的新流。在这个示例中，谓词是 lambda 表达式`e -> e.getGender() == Person.Sex.MALE`。如果对象`e`的`gender`字段的值为`Person.Sex.MALE`，则返回布尔值`true`。因此，在这个示例中，`filter`操作返回一个包含集合`roster`中所有男性成员的流。

+   *终端操作*。终端操作，比如`forEach`，产生一个非流结果，比如一个原始值（比如一个双精度值）、一个集合，或者在`forEach`的情况下，根本没有值。在这个示例中，`forEach`操作的参数是 lambda 表达式`e -> System.out.println(e.getName())`，它在对象`e`上调用`getName`方法。（Java 运行时和编译器推断出对象`e`的类型是`Person`。）

以下示例计算了集合`roster`中所有男性成员的平均年龄，使用了由`filter`、`mapToInt`和`average`聚合操作组成的流水线：

```java
double average = roster
    .stream()
    .filter(p -> p.getGender() == Person.Sex.MALE)
    .mapToInt(Person::getAge)
    .average()
    .getAsDouble();
```

`mapToInt`操作返回一个新的`IntStream`类型的流（这是一个只包含整数值的流）。该操作将其参数中指定的函数应用于特定流中的每个元素。在这个示例中，函数是`Person::getAge`，这是一个返回成员年龄的方法引用。（或者，你可以使用 lambda 表达式`e -> e.getAge()`。）因此，在这个示例中，`mapToInt`操作返回一个包含集合`roster`中所有男性成员年龄的流。

`average`操作计算`IntStream`类型流中包含的元素的平均值。它返回一个`OptionalDouble`类型的对象。如果流不包含任何元素，则`average`操作返回一个空的`OptionalDouble`实例，并调用`getAsDouble`方法会抛出`NoSuchElementException`。JDK 包含许多像`average`这样返回通过组合流内容得到的一个值的终端操作。这些操作被称为*归约操作*；更多信息请参见归约部分。

### 聚合操作和迭代器之间的区别

聚合操作，如`forEach`，看起来像迭代器。然而，它们有几个根本的区别：

+   **它们使用内部迭代**：聚合操作不包含像`next`这样的方法来指示它们处理集合的下一个元素。通过*内部委托*，您的应用程序确定*要*迭代的集合，但 JDK 确定*如何*迭代集合。通过*外部迭代*，您的应用程序确定要迭代的集合以及如何迭代它。然而，外部迭代只能按顺序迭代集合的元素。内部迭代没有这种限制。它可以更容易地利用并行计算，这涉及将问题分解为子问题，同时解决这些问题，然后将子问题的解决方案合并为结果。有关更多信息，请参见并行性部分。

+   **它们从流中处理元素**：聚合操作从流中处理元素，而不是直接从集合中处理。因此，它们也被称为*流操作*。

+   **它们支持行为作为参数**：您可以为大多数聚合操作指定 lambda 表达式作为参数。这使您可以自定义特定聚合操作的行为。
