# Trail: RMI

> 原文：[`docs.oracle.com/javase/tutorial/rmi/index.html`](https://docs.oracle.com/javase/tutorial/rmi/index.html)

Java 远程方法调用（RMI）系统允许在一个 Java 虚拟机中运行的对象调用另一个 Java 虚拟机中运行的对象的方法。RMI 提供了 Java 编程语言编写的程序之间的远程通信。

* * *

**注意：** 如果你要连接到一个现有的 IDL 程序，应该使用 Java IDL 而不是 RMI。

* * *

本教程简要介绍了 RMI 系统，然后演示了一个完整的客户端/服务器示例，该示例利用了 RMI 的独特功能，在运行时加载和执行用户定义的任务。示例中的服务器实现了一个通用的计算引擎，客户端用它来计算 ![圆周率符号](img/18da0a76f9e973138bc9428740982fff.png) 的值。

![trail icon **RMI 应用程序概述**](img/overview.html) 描述了 RMI 系统并列出了其优势。此外，本节介绍了典型的 RMI 应用程序，由服务器和客户端组成，并介绍了重要术语。

![trail icon **编写 RMI 服务器**](img/server.html) 演示了计算引擎服务器的代码。本节将教你如何设计和实现一个 RMI 服务器。

![trail icon **创建客户端程序**](img/client.html) 着眼于一个可能的计算引擎客户端，并用它来说明 RMI 客户端的重要特性。

![trail icon **编译和运行示例**](img/example.html) 展示了如何编译和运行计算引擎服务器及其客户端。
