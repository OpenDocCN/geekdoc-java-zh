# 包命名

> 原文：[`docs.oracle.com/javase/tutorial/java/package/namingpkgs.html`](https://docs.oracle.com/javase/tutorial/java/package/namingpkgs.html)

全球范围内的程序员使用 Java 编程语言编写类和接口，很可能许多程序员会为不同类型使用相同的名称。事实上，前面的例子就是这样做的：它定义了一个 `Rectangle` 类，而 `java.awt` 包中已经有一个 `Rectangle` 类。尽管如此，如果它们位于不同的包中，编译器仍允许两个类具有相同的名称。每个 `Rectangle` 类的完全限定名称包括包名。也就是说，`graphics` 包中的 `Rectangle` 类的完全限定名称是 `graphics.Rectangle`，而 `java.awt` 包中的 `Rectangle` 类的完全限定名称是 `java.awt.Rectangle`。

这种方法很有效，除非两个独立的程序员使用相同的包名。如何避免这个问题？约定。

## 命名约定

包名全部小写以避免与类或接口的名称冲突。

公司使用其反转的互联网域名作为其包名的起始部分例如，`com.example.mypackage` 表示由 `example.com` 的程序员创建的名为 `mypackage` 的包。

公司内部发生的名称冲突需要在公司内部通过约定处理，也许可以在公司名称后面加上地区或项目名称（例如，`com.example.region.mypackage`）。

Java 语言中的包以 `java.` 或 `javax.` 开头。

在某些情况下，互联网域名可能不是有效的包名。如果域名包含连字符或其他特殊字符，如果包名以数字或其他 Java 名称不允许用作 Java 名称开头的字符开头，或者包名包含保留的 Java 关键字，例如 "int"。在这种情况下，建议的约定是添加下划线。例如：

合法化包名

| 域名 | 包名前缀 |
| --- | --- |
| `hyphenated-name.example.org` | `org.example.hyphenated_name` |
| `example.int` | `int_.example` |
| `123name.example.com` | `com.example._123name` |
