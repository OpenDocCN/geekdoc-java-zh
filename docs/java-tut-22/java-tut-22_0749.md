# 课程：远程管理

> 原文：[`docs.oracle.com/javase/tutorial/jmx/remote/index.html`](https://docs.oracle.com/javase/tutorial/jmx/remote/index.html)

JMX API 使您能够通过使用基于 JMX 技术的连接器（JMX 连接器）对资源进行远程管理。 JMX 连接器使 MBean 服务器对远程基于 Java 技术的客户端可访问。 连接器的客户端端口基本上导出与 MBean 服务器相同的接口。

JMX 连接器由连接器客户端和连接器服务器组成。 *连接器服务器* 附加到 MBean 服务器并监听来自客户端的连接请求。 *连接器客户端* 负责与连接器服务器建立连接。 连接器客户端通常位于与连接器服务器不同的 Java 虚拟机（Java VM）中，并且通常在不同的计算机上运行。 JMX API 定义了基于远程方法调用（RMI）的标准连接协议。 此协议使您能够从远程位置将 JMX 客户端连接到 MBean 服务器中的 MBean，并执行对 MBean 的操作，就像这些操作是在本地执行一样。

Java SE 平台提供了一种开箱即用的方式，通过使用 JMX API 的标准 RMI 连接器远程监视应用程序。 开箱即用的 RMI 连接器会自动将应用程序暴露给远程管理，而无需您自己创建专用的远程连接器服务器。 开箱即用的远程管理代理通过使用正确的属性启动 Java 应用程序来激活。 与 JMX 技术兼容的监视和管理应用程序然后可以连接到这些应用程序并远程监视它们。
