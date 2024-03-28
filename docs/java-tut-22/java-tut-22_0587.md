# 区域的范围

> 原文：[`docs.oracle.com/javase/tutorial/i18n/locale/scope.html`](https://docs.oracle.com/javase/tutorial/i18n/locale/scope.html)

Java 平台不要求您在整个程序中使用相同的`Locale`。如果愿意，您可以为程序中的每个区域敏感对象分配不同的`Locale`。这种灵活性使您能够开发多语言应用程序，可以在多种语言中显示信息。

然而，大多数应用程序不是多语言的，它们的区域敏感对象依赖于默认的`Locale`。当 Java 虚拟机启动时由其设置，默认的`Locale`对应于主机平台的区域设置。要确定您的 Java 虚拟机的默认`Locale`，请调用`Locale.getDefault`方法。

* * *

**注意：**

可以独立设置两种用途的默认区域设置：*format*设置用于格式化资源，*display*设置用于菜单和对话框。在 Java SE 7 版本中引入的[`Locale.getDefault(Locale.Category)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getDefault-java.util.Locale.Category-)方法接受一个[`Locale.Category`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.Category.html)参数。将`FORMAT`枚举传递给`getDefault(Locale.Category)`方法会返回用于格式化资源的默认区域设置。类似地，传递`DISPLAY`枚举会返回 UI 使用的默认区域设置。相应的[`setDefault(Locale.Category, Locale)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#setDefault-java.util.Locale.Category-java.util.Locale-)方法允许设置所需类别的区域设置。无参数的`getDefault`方法返回`DISPLAY`的默认值。

在 Windows 平台上，这些默认值根据 Windows 控制面板中的“标准和格式”和“显示语言”设置进行初始化。

* * *

你不应该通过编程方式设置默认的`Locale`，因为它被所有区域敏感类共享。

分布式计算引发了一些有趣的问题。例如，假设您正在设计一个应用服务器，该服务器将接收来自各个国家的客户端的请求。如果每个客户端的`Locale`不同，那么服务器的`Locale`应该是什么？也许服务器是多线程的，每个线程设置为服务的客户端的`Locale`。或者也许服务器和客户端之间传递的所有数据都应该是与区域设置无关的。

你应该采取哪种设计方法？如果可能的话，服务器和客户端之间传递的数据应该是与地区无关的。这样可以简化服务器的设计，让客户端负责以地区敏感的方式显示数据。然而，如果服务器必须以特定地区的形式存储数据，这种方法就行不通了。例如，服务器可能会在不同的数据库列中存储相同数据的西班牙语、英语和法语版本。在这种情况下，服务器可能希望查询客户端的`Locale`，因为`Locale`可能在上次请求之后发生了变化。
