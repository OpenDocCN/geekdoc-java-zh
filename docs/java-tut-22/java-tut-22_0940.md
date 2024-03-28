# 什么是网络接口？

> 原文：[`docs.oracle.com/javase/tutorial/networking/nifs/definition.html`](https://docs.oracle.com/javase/tutorial/networking/nifs/definition.html)

*网络接口*是计算机与私有或公共网络之间的连接点。网络接口通常是网络接口卡（NIC），但不一定要有物理形式。相反，网络接口可以在软件中实现。例如，环回接口（IPv4 的`127.0.0.1`和 IPv6 的`::1`）不是物理设备，而是模拟网络接口的软件部分。环回接口通常用于测试环境。

[`java.net.NetworkInterface`](https://docs.oracle.com/javase/8/docs/api/java/net/NetworkInterface.html)类代表两种类型的接口。

`NetworkInterface`对于具有多个 NIC 的多宿主系统非常有用。使用`NetworkInterface`，您可以指定要用于特定网络活动的 NIC。

例如，假设您有一台配置了两个 NIC 的机器，并且您想向服务器发送数据。您可以这样创建一个套接字：

```java
Socket soc = new java.net.Socket();
soc.connect(new InetSocketAddress(address, port));

```

发送数据时，系统会确定使用哪个接口。但是，如果您有偏好或需要指定要使用的 NIC，您可以查询系统以找到适当的接口并在您想要使用的接口上找到一个地址。当您创建套接字并将其绑定到该地址时，系统将使用相关联的接口。例如：

```java
NetworkInterface nif = NetworkInterface.getByName("bge0");
Enumeration<InetAddress> nifAddresses = nif.getInetAddresses();

Socket soc = new java.net.Socket();
soc.bind(new InetSocketAddress(nifAddresses.nextElement(), 0));
soc.connect(new InetSocketAddress(address, port));

```

您还可以使用`NetworkInterface`来识别要加入多播组的本地接口。例如：

```java
NetworkInterface nif = NetworkInterface.getByName("bge0");
MulticastSocket ms = new MulticastSocket();
ms.joinGroup(new InetSocketAddress(hostname, port), nif);

```

`NetworkInterface`可以与 Java API 一起以许多其他方式使用，超出了此处描述的两种用途。
