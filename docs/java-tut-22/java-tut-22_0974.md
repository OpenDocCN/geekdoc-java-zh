# 命名包

> 原文：[`docs.oracle.com/javase/tutorial/jndi/overview/naming.html`](https://docs.oracle.com/javase/tutorial/jndi/overview/naming.html)

[`javax.naming`](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-summary.html)包含用于访问命名服务的类和接口。

## 上下文

`javax.naming`包定义了一个[`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html)接口，这是查找、绑定/解绑、重命名对象以及创建和销毁子上下文的核心接口。

查找

最常用的操作是[`lookup()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-)。您提供`lookup()`要查找的对象的名称，它将返回绑定到该名称的对象。

绑定

[`listBindings()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#listBindings-javax.naming.Name-)返回一个名称到对象绑定的枚举。绑定是一个包含绑定对象的名称、对象类的名称和对象本身的元组。

列表

[`list()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#list-javax.naming.Name-)类似于`listBindings()`，只是它返回一个包含对象名称和对象类名称的名称枚举。`list()`对于诸如浏览器之类的应用程序很有用，这些应用程序希望发现上下文中绑定的对象的信息，但不需要所有实际对象。虽然`listBindings()`提供了相同的信息，但它可能是一个更昂贵的操作。

名称

`Name`是表示通用名称的接口——一个有序的零个或多个组件的序列。命名系统使用此接口来定义遵循其约定的名称，如命名和目录概念课程中所述。

引用

对象以不同方式存储在命名和目录服务中。引用可能是对象的非常紧凑的表示。

JNDI 定义了[`Reference`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Reference.html)类来表示引用。引用包含有关如何构造对象副本的信息。JNDI 将尝试将从目录查找的引用转换为它们所代表的 Java 对象，以便 JNDI 客户端产生存储在目录中的是 Java 对象的错觉。

## 初始上下文

在 JNDI 中，所有命名和目录操作都是相对于上下文执行的。没有绝对的根。因此，JNDI 定义了一个[`InitialContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html)，它提供了命名和目录操作的起始点。一旦您有了初始上下文，您可以使用它来查找其他上下文和对象。

## 异常

JNDI 定义了一个类层次结构，用于在执行命名和目录操作过程中可能抛出的异常。这个类层次结构的根是[`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html)。对于对特定异常感兴趣的程序，可以捕获异常的相应子类。否则，它们应该捕获`NamingException`。
