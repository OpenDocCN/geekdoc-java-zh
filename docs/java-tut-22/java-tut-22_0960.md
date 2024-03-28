# 将遗留代码转换为使用泛型

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/convert.html`](https://docs.oracle.com/javase/tutorial/extra/generics/convert.html)

之前，我们展示了新代码和旧代码如何互操作。现在，是时候看看更难的问题，即"泛型化"旧代码。

如果决定将旧代码转换为使用泛型，需要仔细考虑如何修改 API。

您需要确保泛型 API 不会过于限制性；它必须继续支持 API 的原始契约。再次考虑一些`java.util.Collection`的示例。预泛型 API 看起来像：

```java
interface Collection {
    public boolean containsAll(Collection c);
    public boolean addAll(Collection c);
}

```

一个天真的泛型尝试可能是以下内容：

```java
interface Collection<E> {

    public boolean containsAll(Collection<E> c);
    public boolean addAll(Collection<E> c);
}

```

虽然这肯定是类型安全的，但它并不符合 API 的原始契约。`containsAll()`方法适用于任何类型的传入集合。只有当传入集合确实只包含`E`的实例时才会成功，但：

+   传入集合的静态类型可能不同，也许是因为调用者不知道传入的集合的确切类型，或者是因为它是一个`Collection<S>`，其中`S`是`E`的子类型。

+   使用不同类型的集合调用`containsAll()`是完全合法的。该例程应该正常工作，返回`false`。

在`addAll()`的情况下，我们应该能够添加任何由`E`的子类型的实例组成的集合。我们看到如何在泛型方法部分正确处理这种情况。

您还需要确保修改后的 API 与旧客户端保持二进制兼容性。这意味着 API 的擦除必须与原始的未泛型化 API 相同。在大多数情况下，这是自然而然的，但也有一些微妙的情况。我们将检查我们遇到的最微妙的情况之一，即方法`Collections.max()`。正如我们在通配符的更多乐趣部分看到的，`max()`的一个合理签名是：

```java
public static <T extends Comparable<? super T>> 
        T max(Collection<T> coll)

```

这很好，除了这个签名的擦除是：

```java
public static Comparable max(Collection coll)

```

这与`max()`的原始签名不同：

```java
public static Object max(Collection coll)

```

当然可以为`max()`指定这个签名，但没有这样做，所有调用`Collections.max()`的旧二进制类文件都依赖于返回`Object`的签名。

我们可以通过在形式类型参数`T`的边界中明确指定一个超类来强制擦除不同。

```java
public static <T extends Object & Comparable<? super T>> 
        T max(Collection<T> coll)

```

这是给类型参数*多重边界*的一个示例，使用语法`T1 & T2 ... & Tn`。具有多重边界的类型变量被认为是边界中列出的所有类型的子类型。当使用多重边界时，边界中提到的第一个类型被用作类型变量的擦除。

最后，我们应该记住`max`只从其输入集合中读取，因此适用于`T`的任何子类型的集合。

这将我们带到 JDK 中实际使用的签名：

```java
public static <T extends Object & Comparable<? super T>> 
        T max(Collection<? extends T> coll)

```

在实践中很少会出现这么复杂的情况，但是专家库设计者在转换现有 API 时应该准备好仔细思考。

另一个需要注意的问题是*协变返回*，即在子类中细化方法的返回类型。在旧的 API 中不应该利用这个特性。为了理解为什么，让我们看一个例子。

假设你原始的 API 形式为：

```java
public class Foo {
    // *Factory. Should create an instance of* 
    // *whatever class it is declared in.*
    public Foo create() {
        ...
    }
}

public class Bar extends Foo {
    // *Actually creates a Bar.*
    public Foo create() {
        ...
    }
}

```

利用协变返回，你将其修改为：

```java
public class Foo {
    // *Factory. Should create an instance of* 
    // *whatever class it is declared in.*
    public Foo create() {
        ...
    }
}

public class Bar extends Foo {
    // *Actually creates a Bar.*
    public Bar create() {
        ...
    }
}

```

现在，假设你的代码的第三方客户写了以下内容：

```java
public class Baz extends Bar {
    // *Actually creates a Baz.*
    public Foo create() {
        ...
    }
}

```

Java 虚拟机不直接支持具有不同返回类型的方法重写。这个特性由编译器支持。因此，除非重新编译类`Baz`，否则它将无法正确重写`Bar`的`create()`方法。此外，`Baz`将需要修改，因为代码将被拒绝——`Baz`中的`create()`的返回类型不是`Bar`中`create()`的返回类型的子类型。
