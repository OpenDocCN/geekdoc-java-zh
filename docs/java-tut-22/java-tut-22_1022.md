# 存储和读取对象

> 原文：[`docs.oracle.com/javase/tutorial/jndi/objects/store.html`](https://docs.oracle.com/javase/tutorial/jndi/objects/store.html)

应用程序和服务可以以不同的方式使用目录来存储和定位对象：

+   存储（对象的副本）对象本身。

+   存储对象的引用。

+   存储描述对象的属性。

一般来说，Java 对象的序列化形式包含对象的状态和对象的引用，以紧凑的寻址信息表示，可用于联系对象。一些示例在查找对象课程中给出。对象的属性用于描述对象的属性；属性可能包括寻址和/或状态信息。

使用这三种方式中的哪一种取决于正在构建的应用程序/系统以及它需要与其他应用程序和系统进行交互的方式，这些应用程序和系统将共享目录中存储的对象。另一个因素是服务提供商和底层目录服务提供的支持。

在将对象存储在目录中时，所有应用程序都使用以下方法之一：

+   [`Context.bind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#bind-javax.naming.Name-java.lang.Object-)

+   [`DirContext.bind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)

+   [`Context.rebind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rebind-javax.naming.Name-java.lang.Object-)

+   [`DirContext.rebind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#rebind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)

应用程序将要存储的对象传递给这些方法之一。然后，根据服务提供商支持的对象类型，对象将被转换为底层目录服务可接受的表示形式。

本课程展示了如何在目录中存储可序列化对象，一旦对象被存储，你可以简单地使用[`lookup()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-)从目录中获取对象的副本，而不管实际存储了什么类型的信息。

你不仅可以通过使用`lookup()`来获取对象，还可以在列出上下文时以及搜索上下文或其子树时获取对象。在所有这些情况下，*对象工厂*可能会参与其中。对象工厂在[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/objects/factory/index.html)中有详细讨论。

对于存储以下对象类型，请参考 JNDI 教程：

+   [Referenceable 对象和 JNDI `Reference`s](https://docs.oracle.com/javase/jndi/tutorial/objects/storing/reference.html)

    添加、替换或移除绑定课程中的 bind()示例使用 Referenceable 对象。

+   [带有属性的对象（`DirContext`）](https://docs.oracle.com/javase/jndi/tutorial/objects/storing/dircontext.html)

+   [RMI（Java 远程方法调用）对象（包括使用 IIOP 的对象）](https://docs.oracle.com/javase/jndi/tutorial/objects/storing/remote.html)

+   [CORBA 对象](https://docs.oracle.com/javase/jndi/tutorial/objects/storing/corba.html)

* * *

**在继续之前：** 要成功运行这些示例，您必须在服务器中关闭模式检查，或者将附带本教程的`Java 模式`添加到服务器中。这项任务通常由目录服务器的管理员执行。有关更多信息，请参阅软件设置课程。

**Windows Active Directory：** [`Context.rebind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rebind-javax.naming.Name-java.lang.Object-) 和 [`DirContext.rebind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#rebind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-) 在 Active Directory 上不起作用，因为这些方法通过读取要更新的条目的属性，删除该条目，然后添加一个包含修改后属性的新条目来工作。Active Directory 返回一些用户无法设置的属性，导致最终添加步骤失败。解决此问题的方法是使用[`DirContext.getAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#getAttributes-javax.naming.Name-) 获取并保存要保留的属性。然后，使用[`DirContext.bind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)将条目删除并添加回来，其中包括保存的属性（以及其他要添加的属性）。
