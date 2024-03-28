# 事件和服务提供者包

> 原文：[`docs.oracle.com/javase/tutorial/jndi/overview/event.html`](https://docs.oracle.com/javase/tutorial/jndi/overview/event.html)

## 事件包

[`javax.naming.event`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/package-summary.html)包含用于支持命名和目录服务中事件通知的类和接口。事件通知在事件通知指南中有详细描述。

事件

一个[`NamingEvent`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamingEvent.html)代表由命名/目录服务生成的事件。事件包含一个*类型*，用于标识事件类型。例如，事件类型被分类为影响命名空间的事件，如“对象添加”，以及不影响命名空间的事件，如“对象更改”。

监听器

一个[`NamingListener`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamingListener.html)是一个监听`NamingEvent`的对象。每种事件类型的类别都有相应类型的`NamingListener`。例如，一个[`NamespaceChangeListener`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamespaceChangeListener.html)代表一个对命名空间更改事件感兴趣的监听器，而一个[`ObjectChangeListener`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/ObjectChangeListener.html)代表一个对对象更改事件感兴趣的监听器。

要接收事件通知，监听器必须注册到[`EventContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventContext.html)或[`EventDirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventDirContext.html)中。一旦注册，当命名/目录服务中发生相应更改时，监听器将接收事件通知。有关事件通知的详细信息可以在[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/beyond/event/index.html)中找到。

## 服务提供者包

[`javax.naming.spi`](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/package-summary.html)包提供了不同命名/目录服务提供者的开发者可以开发和连接其实现的手段，以便从使用 JNDI 的应用程序中访问相应服务。

插件架构

`javax.naming.spi`包允许动态地插入不同的实现。这些实现包括初始上下文和可以从初始上下文访问的上下文。

Java 对象支持

`javax.naming.spi`包支持 lookup 及其相关方法的实现者返回对于 Java 程序员而言自然且直观的 Java 对象。例如，如果你从目录中查找打印机名称，那么你很可能期望得到一个打印机对象来进行操作。这种支持以 object factories 的形式提供。

这个包还提供了支持进行相反操作的功能。也就是说，[`Context.bind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#bind-javax.naming.Name-java.lang.Object-)及其相关方法的实现者可以接受 Java 对象，并将这些对象存储在底层命名/目录服务可接受的格式中。这种支持以 state factories 的形式提供。

多个命名系统（联邦）

JNDI 操作允许应用程序提供跨多个命名系统的名称。在完成操作的过程中，一个服务提供者可能需要与另一个服务提供者交互，例如将操作传递给下一个命名系统继续进行。这个包提供了不同提供者合作完成 JNDI 操作的支持。

有关服务提供者机制的详细信息可以在[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/provider/index.html)中找到。
