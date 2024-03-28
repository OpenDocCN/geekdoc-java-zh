# 介绍

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/intro.html`](https://docs.oracle.com/javase/tutorial/extra/generics/intro.html)

JDK 5.0 引入了 Java 编程语言的几个新扩展之一就是*泛型*。

本教程是关于泛型的介绍。您可能熟悉其他语言中类似构造的概念，尤其是 C++的模板。如果是这样，您会发现有相似之处，也有重要的区别。如果您对其他地方类似的构造不熟悉，那就更好了；您可以从头开始，无需消除任何误解。

泛型允许您对类型进行抽象。最常见的例子是容器类型，比如在 Collections 层次结构中的类型。

这是一个典型的使用方式：

```java
List myIntList = new LinkedList(); // 1
myIntList.add(new Integer(0)); // 2
Integer x = (Integer) myIntList.iterator().next(); // 3        

```

第 3 行的强制类型转换略显烦人。通常，程序员知道特定列表中放入了什么类型的数据。然而，强制类型转换是必不可少的。编译器只能保证迭代器返回一个`Object`。为了确保将其分配给类型为`Integer`的变量是类型安全的，需要进行强制类型转换。

当然，强制类型转换不仅引入了混乱。它还引入了运行时错误的可能性，因为程序员可能会犯错。

如果程序员能够真正表达他们的意图，并将列表标记为仅包含特定数据类型，会怎么样？这就是泛型背后的核心思想。以下是使用泛型给出的程序片段的一个版本：

```java
List<Integer> 
    myIntList = new LinkedList<Integer>(); // 1'
myIntList.add(new Integer(0)); // 2'
Integer x = myIntList.iterator().next(); // 3'

```

注意变量`myIntList`的类型声明。它指定这不仅仅是一个任意的`List`，而是一个`Integer`的`List`，写作`List<Integer>`。我们说`List`是一个接受*类型参数*的泛型接口--在这种情况下是`Integer`。我们在创建列表对象时也指定了类型参数。

注意，第 3 行的强制类型转换已经消失了。

现在，您可能会认为我们所做的只是将混乱的地方移动了一下。在第 3 行进行`Integer`的强制类型转换，我们在第 1 行将`Integer`作为类型参数。然而，这里有一个非常大的区别。编译器现在可以在编译时检查程序的类型正确性。当我们说`myIntList`声明为类型`List<Integer>`时，这告诉我们关于变量`myIntList`的一些信息，无论何时何地使用它，编译器都会保证这一点。相比之下，强制类型转换告诉我们程序员认为在代码中的某个单一点上是正确的。

整体效果，尤其是在大型程序中，是提高了可读性和健壮性。
