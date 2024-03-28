# 字符和字符串总结

> 原文：[`docs.oracle.com/javase/tutorial/java/data/stringsummary.html`](https://docs.oracle.com/javase/tutorial/java/data/stringsummary.html)

大多数情况下，如果您使用单个字符值，您将使用基本的`char`类型。然而，有时您需要将 char 用作对象例如，作为期望对象的方法参数。Java 编程语言为此提供了一个*包装*类，将`char`包装在`Character`对象中。`Character`类型的对象包含一个类型为`char`的单个字段。这个[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html)类还提供了许多有用的类（即静态）方法来操作字符。

字符串是字符序列，在 Java 编程中被广泛使用。在 Java 编程语言中，字符串是对象。[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)类有 60 多个方法和 13 个构造函数。

最常见的是，您可以使用类似于以下语句创建一个字符串

```java
String s = "Hello world!";

```

而不是使用其中一个`String`构造函数。

`String`类有许多方法可以查找和检索子字符串；然后可以使用`+`连接运算符将它们轻松重新组装成新的字符串。

`String`类还包括许多实用方法，其中包括`split()`、`toLowerCase()`、`toUpperCase()`和`valueOf()`。后者方法在将用户输入的字符串转换为数字时是不可或缺的。`Number`子类还有将字符串转换为数字以及反之的方法。

除了`String`类之外，还有一个[`StringBuilder`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html)类。与字符串一起工作相比，使用`StringBuilder`对象有时可能更有效率。`StringBuilder`类提供了一些对字符串有用的方法，其中包括`reverse()`。然而，总的来说，`String`类具有更广泛的方法。

可以使用`StringBuilder`构造函数将字符串转换为字符串构建器。可以使用`toString()`方法将字符串构建器转换为字符串。
