# 概述

> 原文：[`docs.oracle.com/javase/tutorial/sdp/sockets/overview.html`](https://docs.oracle.com/javase/tutorial/sdp/sockets/overview.html)

SDP 支持本质上是一种 TCP 绕过技术。

当启用 SDP 并且应用程序尝试打开 TCP 连接时，TCP 机制将被绕过，通信直接传输到 IB 网络。例如，当您的应用程序尝试绑定到 TCP 地址时，基础软件将根据配置文件中的信息决定是否应重新绑定到 SDP 协议。此过程可以在绑定过程或连接过程中发生（但每个套接字仅发生一次）。

在您的代码中利用 SDP 协议无需进行 API 更改：实现是透明的，并受经典网络(`java.net`)和新 I/O(`java.nio.channels`)包的支持。查看支持的 Java API 部分，了解支持 SDP 协议的类列表。

SDP 支持默认情况下是禁用的。启用 SDP 支持的步骤如下：

+   创建一个 SDP 配置文件。

+   设置指定配置文件位置的系统属性。
