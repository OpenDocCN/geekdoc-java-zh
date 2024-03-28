# 定义简单的通用类型

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/simple.html`](https://docs.oracle.com/javase/tutorial/extra/generics/simple.html)

这里是包`java.util`中接口`List`和`Iterator`的定义的一个小节选：

```java
public interface List <E> {
    void add(E x);
    Iterator<E> iterator();
}

public interface Iterator<E> {
    E next();
    boolean hasNext();
}

```

这段代码应该都很熟悉，除了尖括号中的内容。那些是接口`List`和`Iterator`的*形式类型参数*的声明。

类型参数可以在通用声明中的几乎任何地方使用，就像你会使用普通类型一样（尽管有一些重要的限制；请参阅细则部分）。

在介绍中，我们看到了`List`的*调用*，比如`List<Integer>`。在调用（通常称为*参数化类型*）中，所有形式类型参数（在本例中为`E`）的所有出现都被*实际类型参数*（在本例中为`Integer`）替换。

你可能会想象`List<Integer>`代表了`List`的一个版本，其中`E`已经被`Integer`统一替换：

```java
public interface IntegerList {
    void add(Integer x);
    Iterator<Integer> iterator();
}

```

这种直觉可能有所帮助，但也是误导的。

这很有帮助，因为参数化类型`List<Integer>`确实有看起来像这个展开的方法。

这是误导的，因为通用的声明实际上从未以这种方式展开。代码中没有多个副本——不在源代码中，也不在二进制代码中，也不在磁盘上，也不在内存中。如果您是 C++程序员，您会明白这与 C++模板非常不同。

通用类型声明只编译一次，并转换为单个类文件，就像普通类或接口声明一样。

类型参数类似于方法或构造函数中使用的普通参数。就像方法有描述其操作的值种类的*形式值参数*一样，通用声明有形式类型参数。当调用方法时，*实际参数*被替换为形式参数，并且方法体被评估。当调用通用声明时，实际类型参数被替换为形式类型参数。

关于命名约定的说明。我们建议您为形式类型参数使用简洁（如果可能的话是单个字符）但富有启发性的名称。最好避免在这些名称中使用小写字符，这样可以轻松区分形式类型参数和普通类和接口。许多容器类型使用`E`，表示元素，就像上面的例子一样。我们将在后面的例子中看到一些额外的约定。
