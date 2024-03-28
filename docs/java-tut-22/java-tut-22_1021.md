# 课程：目录中的 Java 对象

> 原文：[`docs.oracle.com/javase/tutorial/jndi/objects/index.html`](https://docs.oracle.com/javase/tutorial/jndi/objects/index.html)

传统上，目录被用来存储数据。用户和程序员将目录视为包含一组属性的目录条目的层次结构。您可以从目录中查找条目并提取感兴趣的属性。

对于用 Java 编程语言编写的应用程序，Java 对象有时可能会在应用程序之间共享。对于这样的应用程序，能够将目录用作 Java 对象的存储库是有意义的。目录为分布在网络上的 Java 应用程序提供了一个集中管理的、可能是复制的服务。例如，应用程序服务器可能使用目录来注册代表其管理的服务的对象，以便客户端稍后可以搜索目录以定位所需的服务。JNDI 用作服务目录的示例是 Apache DS。有关更多信息，请访问[Apache Directory](http://directory.apache.org/)。

JNDI 提供了目录的面向对象视图，从而允许将 Java 对象添加到目录中并从中检索，而无需客户端管理数据表示问题。本课程讨论了在基本水平上使用目录存储和检索 Java 对象。JNDI 提供了用于创建和存储从目录访问的对象的对象和状态工厂。

## 对象工厂

对象工厂是对象的生产者。它接受有关如何创建对象的一些信息，例如引用，然后返回该对象的实例。有关对象工厂和对象在目录中存储的格式的详细信息，请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/objects/factory/index.html)。

## 状态工厂

状态工厂将一个对象转换为另一个对象。输入是对象和可选属性，提供给 Context.bind()，输出是另一个对象和可选属性，将存储在底层命名服务或目录中。有关状态工厂以及如何编写自己的状态工厂的详细信息，请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/objects/state/index.html)。

课程的下一部分讨论了如何访问目录中的对象。它描述了可序列化对象如何存储和读取在目录中。有关其他类型的对象，请查看[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/objects/index.html)。
