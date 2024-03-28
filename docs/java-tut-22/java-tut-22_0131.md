# 类型擦除

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/erasure.html`](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)

泛型被引入 Java 语言，以在编译时提供更严格的类型检查，并支持泛型编程。为了实现泛型，Java 编译器对其应用类型擦除：

+   将泛型类型中的所有类型参数替换为它们的边界或`Object`（如果类型参数是无界的）。因此生成的字节码只包含普通类、接口和方法。

+   如有必要，插入类型转换以保持类型安全。

+   生成桥接方法以保留扩展泛型类型中的多态性。

类型擦除确保为参数化类型不会创建新类；因此，泛型不会产生运行时开销。
