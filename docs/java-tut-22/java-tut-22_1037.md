# 启用 SDP 协议

> 原文：[`docs.oracle.com/javase/tutorial/sdp/sockets/enable.html`](https://docs.oracle.com/javase/tutorial/sdp/sockets/enable.html)

SDP 支持默认情况下是禁用的。要启用 SDP 支持，请设置`com.sun.sdp.conf`系统属性，提供配置文件的位置。以下示例启动一个使用名为`sdp.conf`的配置文件的应用程序：

```java
% java -Dcom.sun.sdp.conf=sdp.conf -Djava.net.preferIPv4Stack=true  *ExampleApplication*

```

*ExampleApplication*指的是试图连接到 IB 适配器的客户端应用程序。

请注意，此示例指定另一个系统属性`java.net.preferIPv4Stack`。有关为什么使用此属性的更多信息，请参见 Issues 部分。
