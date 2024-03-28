# 泛型和子类型

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/subtype.html`](https://docs.oracle.com/javase/tutorial/extra/generics/subtype.html)

让我们测试一下你对泛型的理解。以下代码片段是否合法？

```java
List<String> ls = new ArrayList<String>(); // 1
List<Object> lo = ls; // 2 

```

第 1 行肯定是合法的。问题的棘手部分在于第 2 行。这归结为一个问题：`String`的`List`是否是`Object`的`List`。大多数人本能地回答：“当然！”

好吧，看看接下来的几行：

```java
lo.add(new Object()); // 3
String s = ls.get(0); // 4: Attempts to assign an Object to a String!

```

在这里，我们给`ls`和`lo`取了别名。通过别名`lo`访问`String`的列表`ls`，我们可以向其中插入任意对象。结果`ls`不再只包含`String`，当我们尝试从中取出东西时，会得到一个不愉快的惊喜。

Java 编译器当然会阻止这种情况发生。第 2 行将导致编译时错误。

一般来说，如果`Foo`是`Bar`的子类型（子类或子接口），而`G`是某个泛型类型声明，那么`G<Foo>`不是`G<Bar>`的子类型。这可能是你需要了解的关于泛型最困难的事情，因为它违背了我们根深蒂固的直觉。

我们不应该假设集合不会改变。我们的直觉可能会让我们认为这些东西是不可变的。

例如，如果机动车管理部门向人口普查局提供驾驶员名单，这似乎是合理的。我们认为`List<Driver>`是`List<Person>`，假设`Driver`是`Person`的子类型。实际上，传递的是驾驶员注册表的**副本**。否则，人口普查局可能会将不是驾驶员的新人加入列表，从而破坏了机动车管理部门的记录。

为了应对这种情况，考虑更灵活的泛型类型是很有用的。到目前为止，我们看到的规则相当严格。
