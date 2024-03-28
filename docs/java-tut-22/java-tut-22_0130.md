# 通配符使用准则

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html`](https://docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html)

在学习如何使用泛型编程时，确定何时使用上界通配符和何时使用下界通配符是更令人困惑的方面之一。本页面提供了一些在设计代码时遵循的准则。

在本讨论中，将变量视为提供两种功能之一是有帮助的：

**一个“In”变量**

一个“in”变量向代码提供数据。想象一个带有两个参数的复制方法：`copy(src, dest)`。`src`参数提供要复制的数据，因此它是“in”参数。

**一个“Out”变量**

一个“out”变量保存数据以供其他地方使用。在复制示例中，`copy(src, dest)`，`dest`参数接受数据，因此它是“out”参数。

当然，有些变量既用于“in”又用于“out” —— 这种情况也在准则中有所涉及。

在决定是否使用通配符以及何种类型的通配符适合时，可以使用“in”和“out”原则。以下列表提供了要遵循的准则：

* * *

**通配符准则：**

+   使用`extends`关键字定义具有上界通配符的“in”变量。

+   使用`super`关键字定义具有下界通配符的“out”变量。

+   在“in”变量可以使用在`Object`类中定义的方法访问的情况下，使用无界通配符。

+   在代码需要将变量作为“in”和“out”变量访问的情况下，不要使用通配符。

* * *

这些准则不适用于方法的返回类型。应避免将通配符用作返回类型，因为这会强迫使用代码的程序员处理通配符。

由`List<? extends ...>`定义的列表可以非正式地被视为只读的，但这并不是一个严格的保证。假设你有以下两个类：

```java
class NaturalNumber {

    private int i;

    public NaturalNumber(int i) { this.i = i; }
    // ...
}

class EvenNumber extends NaturalNumber {

    public EvenNumber(int i) { super(i); }
    // ...
}

```

考虑以下代码：

```java
List<EvenNumber> le = new ArrayList<>();
List<? extends NaturalNumber> ln = le;
ln.add(new NaturalNumber(35));  // compile-time error

```

因为`List<EvenNumber>`是`List<? extends NaturalNumber>`的子类型，所以你可以将`le`赋给`ln`。但你不能使用`ln`向偶数列表添加自然数。列表上的以下操作是可能的：

+   你可以添加`null`。

+   你可以调用`clear`。

+   你可以获取迭代器并调用`remove`。

+   你可以捕获通配符并写入你从列表中读取的元素。

你可以看到由`List<? extends NaturalNumber>`定义的列表在严格意义上并不是只读的，但你可能会这样认为，因为你不能在列表中存储新元素或更改现有元素。
