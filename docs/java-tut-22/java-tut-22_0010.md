# 教程：深入了解 "Hello World!" 应用程序

> 原文：[`docs.oracle.com/javase/tutorial/getStarted/application/index.html`](https://docs.oracle.com/javase/tutorial/getStarted/application/index.html)

现在你已经看到了 "Hello World!" 应用程序（甚至可能已经编译并运行了它），你可能想知道它是如何工作的。这里再次是它的代码：

```java

class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}

```

"Hello World!" 应用程序由三个主要组件组成：源代码注释、`HelloWorldApp` 类定义 和 `main` 方法。以下解释将为您提供对代码的基本理解，但更深层次的含义只有在您完成阅读本教程的其余部分后才会显现。

## 源代码注释

下面的加粗文本定义了 "Hello World!" 应用程序的*注释*：

```java
/**
 * The HelloWorldApp class implements an application that
 * simply prints "Hello World!" to standard output.
 */
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}

```

注释被编译器忽略，但对其他程序员有用。Java 编程语言支持三种类型的注释：

`/* *文本* */`

编译器忽略从 `/*` 到 `*/` 的所有内容。

`/** *文档* */`

这表示一个文档注释（*doc comment*）。编译器忽略这种类型的注释，就像它忽略使用 `/*` 和 `*/` 的注释一样。`javadoc` 工具在准备自动生成的文档时使用文档注释。有关 `javadoc` 的更多信息，请参阅[Javadoc™ 工具文档](https://docs.oracle.com/javase/8/docs/technotes/guides/javadoc/index.html)。

`// *文本*`

编译器忽略从 `//` 到行尾的所有内容。

## `HelloWorldApp` 类定义

下面的加粗文本开始了 "Hello World!" 应用程序的类定义块：

```java
/**
 * The HelloWorldApp class implements an application that
 * simply displays "Hello World!" to the standard output.
 */
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}

```

如上所示，类定义的最基本形式是：

```java
class *name* {
    . . .
}

```

关键字 `class` 开始了名为 `name` 的类的类定义，每个类的代码都出现在上面加粗的大括号之间。第二章概述了一般的类，第四章详细讨论了类。现在知道每个应用程序都以类定义开始就足够了。

## `main` 方法

下面的加粗文本开始了 `main` 方法的定义：

```java
/**
 * The HelloWorldApp class implements an application that
 * simply displays "Hello World!" to the standard output.
 */
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); //Display the string.
    }
}

```

在 Java 编程语言中，每个应用程序必须包含一个签名为 `main` 的方法：

```java
public static void main(String[] args)

```

修饰符 `public` 和 `static` 可以以任何顺序编写（`public static` 或 `static public`），但约定是使用如上所示的 `public static`。你可以将参数命名为任何你想要的东西，但大多数程序员选择 "args" 或 "argv"。

`main` 方法类似于 C 和 C++ 中的 `main` 函数；它是应用程序的入口点，并随后将调用程序所需的所有其他方法。

`main` 方法接受一个参数：类型为 `String` 的元素数组。

```java
public static void main(String[] args)

```

这个数组是运行时系统向你的应用程序传递信息的机制。例如：

```java
java *MyApp* *arg1* *arg2*

```

数组中的每个字符串被称为*命令行参数*。命令行参数让用户在不重新编译应用程序的情况下影响其操作。例如，一个排序程序可能允许用户通过以下命令行参数指定按降序排序数据：

```java
-descending

```

"Hello World!" 应用程序忽略了它的命令行参数，但你应该意识到这些参数确实存在。

最后，这行代码：

```java
System.out.println("Hello World!");

```

使用核心库中的`System`类将"Hello World!"消息打印到标准输出。本教程的其余部分将讨论该库的部分内容（也称为"应用程序编程接口"或"API"）。
