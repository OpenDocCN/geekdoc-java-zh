# 支持的 Java API

> 原文：[`docs.oracle.com/javase/tutorial/sdp/sockets/supported.html`](https://docs.oracle.com/javase/tutorial/sdp/sockets/supported.html)

所有使用 TCP 的 API 都可以使用 SDP，特别包括以下类：

+   `java.net` 包

    +   `Socket`

    +   `ServerSocket`

+   `java.nio.channels` 包：

    +   `SocketChannel`

    +   `ServerSocketChannel`

    +   `AsynchronousSocketChannel`

    +   `AsynchronousServerSocketChannel`

当启用 SDP 支持时，它会在不需要对您的代码进行任何更改的情况下正常工作。不需要编译。然而，重要的是要知道套接字只绑定一次。连接是隐式绑定。因此，如果套接字之前没有绑定并且调用了 `connect`，则绑定将在那个时候发生。

例如，考虑以下代码片段：

```java
AsynchronousSocketChannel ch = AsynchronousSocketChannel.open();
ch.bind(local);
Future<Void> result = ch.connect(remote);

```

在此代码片段中，当在套接字上调用 `bind` 时，异步套接字通道将绑定到本地 TCP 地址。然后，代码尝试使用相同的套接字连接到远程地址。如果远程地址使用配置文件中指定的 InfiniBand，则连接不会转换为 SDP，因为套接字先前已绑定。
