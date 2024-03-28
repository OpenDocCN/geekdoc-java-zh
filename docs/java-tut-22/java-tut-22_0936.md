# 什么是数据报？

> 原文：[`docs.oracle.com/javase/tutorial/networking/datagrams/definition.html`](https://docs.oracle.com/javase/tutorial/networking/datagrams/definition.html)

客户端和服务器通过可靠的通道（如 TCP 套接字）进行通信，它们之间有一个专用的点对点通道，或者至少有这种幻觉。为了通信，它们建立连接，传输数据，然后关闭连接。所有通过通道发送的数据都按照发送顺序接收。这由通道保证。

相反，通过数据报通信的应用程序发送和接收完全独立的信息包。这些客户端和服务器没有也不需要专用的点对点通道。数据报的传递到目的地不被保证。它们的到达顺序也不被保证。

* * *

**定义：**

一个*数据报*是独立的、自包含的消息，通过网络发送，其到达、到达时间和内容都不被保证。

* * *

`java.net`包含三个类，帮助你编写使用数据报在网络上传输和接收数据包的 Java 程序：[`DatagramSocket`](https://docs.oracle.com/javase/8/docs/api/java/net/DatagramSocket.html)、[`DatagramPacket`](https://docs.oracle.com/javase/8/docs/api/java/net/DatagramPacket.html)和[`MulticastSocket`](https://docs.oracle.com/javase/8/docs/api/java/net/MulticastSocket.html)。一个应用程序可以通过`DatagramSocket`发送和接收`DatagramPacket`。此外，`DatagramPacket`可以广播到多个接收者，所有接收者都监听`MulticastSocket`。
