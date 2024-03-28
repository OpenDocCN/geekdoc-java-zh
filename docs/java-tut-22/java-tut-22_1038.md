# 调试 SDP

> 原文：[`docs.oracle.com/javase/tutorial/sdp/sockets/debug.html`](https://docs.oracle.com/javase/tutorial/sdp/sockets/debug.html)

你可以通过使用`-Dcom.sun.sdp.debug`标志来启用调试消息。如果你指定一个文件，消息将被输出到该文件。否则，消息将被打印到标准输出。

这个第一个示例展示了打印到标准输出的示例消息：

```java
% java -Dcom.sun.sdp.conf=sdp.conf -Dcom.sun.sdp.debug *ExampleApplicaton*
BIND to 192.0.2.1:5000 (socket converted to SDP protocol)
CONNECT to 129.156.232.160:80 (no match)
CONNECT to 192.0.2.2:80 (socket converted to SDP protocol)

```

这个第二个示例展示了输出重定向到名为`debug.log`的文件：

```java
% java -Dcom.sun.sdp.conf=sdp.conf -Dcom.sun.sdp.debug=debug.log *ExampleApplication* 
[1] 27310
% tail -f debug.log
BIND to 192.0.2.1:5000 (socket converted to SDP protocol)

```
