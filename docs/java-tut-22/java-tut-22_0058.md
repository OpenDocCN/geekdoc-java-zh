# 初始化字段

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/initial.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/initial.html)

正如你所见，你通常可以在声明中为字段提供初始值：

```java
public class BedAndBreakfast {

    // initialize to 10
    public static int capacity = 10;

    // initialize to false
    private boolean full = false;
}

```

当初始化值可用且初始化可以放在一行时，这种方式效果很好。然而，这种初始化方式由于其简单性而有一些限制。如果初始化需要一些逻辑（例如，错误处理或使用`for`循环填充复杂数组），简单赋值是不够的。实例变量可以在构造函数中初始化，可以在那里使用错误处理或其他逻辑。为了为类变量提供相同的功能，Java 编程语言包括*静态初始化块*。

* * *

**注意：**在类定义的开头声明字段并不是必需的，尽管这是最常见的做法。只需要在使用之前声明和初始化它们即可。

* * *

## 静态初始化块

*静态初始化块*是一个普通的代码块，用大括号`{ }`括起来，并在`static`关键字之前。这里是一个示例：

```java
static {
    // whatever code is needed for initialization goes here
}

```

一个类可以有任意数量的静态初始化块，并且它们可以出现在类体的任何位置。运行时系统保证静态初始化块按照它们在源代码中出现的顺序调用。

还有一种替代静态块的方法  你可以编写一个私有静态方法：

```java
class Whatever {
    public static varType myVar = initializeClassVariable();

    private static varType initializeClassVariable() {

        // initialization code goes here
    }
}

```

私有静态方法的优势在于，如果需要重新初始化类变量，它们可以在以后被重用。

## 初始化实例成员

通常，你会将代码放在构造函数中初始化实例变量。有两种替代方法可以用来初始化实例变量：初始化块和 final 方法。

实例变量的初始化块看起来就像静态初始化块，但没有`static`关键字：

```java
{
    // whatever code is needed for initialization goes here
}

```

Java 编译器将初始化块复制到每个构造函数中。因此，这种方法可以用于在多个构造函数之间共享一段代码。

*final 方法*不能在子类中被重写。这在接口和继承的课程中有讨论。这里是使用 final 方法初始化实例变量的示例：

```java
class Whatever {
    private varType myVar = initializeInstanceVariable();

    protected final varType initializeInstanceVariable() {

        // initialization code goes here
    }
}

```

如果子类可能希望重用初始化方法，则这是特别有用的。该方法是 final 的，因为在实例初始化期间调用非 final 方法可能会导致问题。
