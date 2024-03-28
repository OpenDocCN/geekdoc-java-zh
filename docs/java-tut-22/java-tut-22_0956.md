# 与遗留代码互操作

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/legacy.html`](https://docs.oracle.com/javase/tutorial/extra/generics/legacy.html)

到目前为止，我们所有的例子都假设了一个理想化的世界，在这个世界中，每个人都在使用支持泛型的最新版本的 Java 编程语言。

然而，现实情况并非如此。数百万行代码是用早期版本的语言编写的，它们不会一夜之间全部转换。

稍后，在将遗留代码转换为使用泛型部分，我们将解决将旧代码转换为使用泛型的问题。在本节中，我们将专注于一个更简单的问题：如何使遗留代码和通用代码互操作？这个问题有两个部分：在通用代码中使用遗留代码和在遗留代码中使用通用代码。

## 在通用代码中使用遗留代码

如何在使用自己的代码时使用旧代码，同时仍享受泛型的好处？

举个例子，假设你想使用包 `com.Example.widgets`。Example.com 的人们推出了一个用于库存控制的系统，其亮点如下所示：

```java
package com.Example.widgets;

public interface Part {...}

public class Inventory {
    /**
     * Adds a new Assembly to the inventory database.
     * The assembly is given the name name, and 
     * consists of a set parts specified by parts. 
     * All elements of the collection parts
     * must support the Part interface.
     **/ 
    public static void addAssembly(String name, Collection parts) {...}
    public static Assembly getAssembly(String name) {...}
}

public interface Assembly {
    // *Returns a collection of Parts*
    Collection getParts();
}

```

现在，你想添加新代码，使用上面的 API。最好确保你总是使用正确的参数调用 `addAssembly()` - 也就是说，你传入的集合确实是 `Part` 的 `Collection`。当然，泛型正是为此而设计的：

```java
package com.mycompany.inventory;

import com.Example.widgets.*;

public class Blade implements Part {
    ...
}

public class Guillotine implements Part {
}

public class Main {
    public static void main(String[] args) {
        Collection<Part> c = new ArrayList<Part>();
        c.add(new Guillotine()) ;
        c.add(new Blade());
        Inventory.addAssembly("thingee", c);
        Collection<Part> k = Inventory.getAssembly("thingee").getParts();
    }
}

```

当我们调用 `addAssembly` 时，它期望第二个参数是 `Collection` 类型。实际参数是 `Collection<Part>` 类型。这样也能工作，但为什么呢？毕竟，大多数集合不包含 `Part` 对象，因此一般情况下，编译器无法知道 `Collection` 类型引用的是什么类型的集合。

在适当的通用代码中，`Collection` 总是伴随着一个类型参数。当像 `Collection` 这样的通用类型在没有类型参数的情况下使用时，它被称为*原始类型*。

大多数人的第一反应是 `Collection` 真的意味着 `Collection<Object>`。然而，正如我们之前看到的，将 `Collection<Part>` 传递到需要 `Collection<Object>` 的地方是不安全的。更准确地说，类型 `Collection` 表示某种未知类型的集合，就像 `Collection<?>` 一样。

但等等，这也不对！考虑一下对 `getParts()` 的调用，它返回一个 `Collection`。然后将其赋给 `k`，它是一个 `Collection<Part>`。如果调用的结果是 `Collection<?>`，那么赋值就会出错。

实际上，这种赋值是合法的，但会生成一个*未经检查的警告*。这个警告是必要的，因为事实上编译器无法保证其正确性。我们无法检查 `getAssembly()` 中的遗留代码，以确保返回的确实是一个 `Part` 集合。代码中使用的类型是 `Collection`，可以合法地向这样的集合中插入各种对象。

那么，这不应该是一个错误吗？从理论上讲，是的；但从实际上讲，如果通用代码要调用遗留代码，这必须被允许。这取决于你，程序员，要确信在这种情况下，赋值是安全的，因为`getAssembly()`的契约规定它返回一个`Part`的集合，即使类型签名没有显示这一点。

因此，原始类型非常类似于通配符类型，但它们的类型检查不那么严格。这是一个故意的设计决定，允许泛型与现有的遗留代码进行交互。

从通用代码调用遗留代码本质上是危险的；一旦将通用代码与非通用遗留代码混合，通常提供的所有通用类型系统的安全保证都将无效。然而，你仍然比根本不使用泛型要好。至少你知道你这边的代码是一致的。

目前，非泛型代码比泛型代码要多得多，而且不可避免地会出现必须混合使用它们的情况。

如果你发现必须混合使用遗留代码和通用代码，请密切关注未经检查的警告。仔细考虑如何证明引发警告的代码的安全性。

如果你仍然犯了错误，导致警告的代码确实不安全，会发生什么？让我们看看这种情况。在这个过程中，我们将深入了解编译器的工作原理。

## 擦除和转换

```java
public String loophole(Integer x) {
    List<String> ys = new LinkedList<String>();
    List xs = ys;
    xs.add(x); // *Compile-time unchecked warning*
    return ys.iterator().next();
}

```

在这里，我们给字符串列表和普通旧列表取了别名。我们将一个`Integer`插入列表，并尝试提取一个`String`。这显然是错误的。如果我们忽略警告并尝试执行此代码，它将在我们尝试使用错误类型的地方失败。在运行时，此代码的行为如下：

```java
public String loophole(Integer x) {
    List ys = new LinkedList;
    List xs = ys;
    xs.add(x); 
    return(String) ys.iterator().next(); // *run time error*
}

```

当我们从列表中提取一个元素，并尝试将其强制转换为`String`以将其视为字符串时，我们将收到`ClassCastException`。与`loophole()`的泛型版本发生的事情完全相同。

这是因为，泛型是由 Java 编译器实现的一种称为*擦除*的前端转换。你（几乎）可以将其视为源到源的转换，其中`loophole()`的泛型版本转换为非泛型版本。

因此，**即使存在未经检查的警告，Java 虚拟机的类型安全性和完整性也永远不会受到威胁**。

基本上，擦除会消除（或*擦除*）所有泛型类型信息。所有尖括号之间的类型信息都被丢弃，因此，例如，像`List<String>`这样的参数化类型被转换为`List`。所有类型变量的剩余用法都被替换为类型变量的上界（通常为`Object`）。并且，每当生成的代码不符合类型时，都会插入到适当类型的强制转换，就像`loophole`的最后一行一样。

擦除的全部细节超出了本教程的范围，但我们刚刚给出的简单描述并不离谱。了解一些关于这个是很有好处的，特别是如果您想要做一些更复杂的事情，比如将现有 API 转换为使用泛型（参见将旧代码转换为使用泛型部分），或者只是想了解为什么事情是这样的。

## 在旧代码中使用泛型代码

现在让我们考虑相反的情况。想象一下，Example.com 选择将他们的 API 转换为使用泛型，但是一些客户端还没有这样做。现在代码看起来像这样：

```java
package com.Example.widgets;

public interface Part { 
    ...
}

public class Inventory {
    /**
     * Adds a new Assembly to the inventory database.
     * The assembly is given the name name, and 
     * consists of a set parts specified by parts. 
     * All elements of the collection parts
     * must support the Part interface.
     **/ 
    public static void addAssembly(String name, Collection<Part> parts) {...}
    public static Assembly getAssembly(String name) {...}
}

public interface Assembly {
    // *Returns a collection of Parts*
    Collection<Part> getParts();
}

```

客户端代码如下：

```java
package com.mycompany.inventory;

import com.Example.widgets.*;

public class Blade implements Part {
...
}

public class Guillotine implements Part {
}

public class Main {
    public static void main(String[] args) {
        Collection c = new ArrayList();
        c.add(new Guillotine()) ;
        c.add(new Blade());

        // *1: unchecked warning*
        Inventory.addAssembly("thingee", c);

        Collection k = Inventory.getAssembly("thingee").getParts();
    }
}

```

客户端代码是在引入泛型之前编写的，但它使用了`com.Example.widgets`包和集合库，两者都使用了泛型类型。客户端代码中所有泛型类型声明的使用都是原始类型。

第 1 行生成了一个未经检查的警告，因为一个原始`Collection`被传递到一个期望`Part`集合的`Collection`位置，编译器无法确保原始`Collection`确实是`Part`集合。

作为一种替代方案，您可以使用源 1.4 标志编译客户端代码，确保不会生成任何警告。然而，在这种情况下，您将无法使用 JDK 5.0 引入的任何新语言特性。

* * *
