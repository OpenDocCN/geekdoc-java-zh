# SDP 的技术问题

> 原文：[`docs.oracle.com/javase/tutorial/sdp/sockets/issues.html`](https://docs.oracle.com/javase/tutorial/sdp/sockets/issues.html)

+   IPv4 和 IPv6 不兼容

    互联网协议第 4 版（IPv4）长期以来一直是 Internet 协议（IP）的行业标准版本，用于在 Internet 上传输数据。互联网协议第 6 版（IPv6）是下一代 Internet 层协议。今天仍在使用这两个版本的 IP。

    IPv4 地址是 32 位长，以十进制格式编写，并用句点分隔。IPv6 地址是 128 位长，以十六进制格式编写，并用冒号分隔。IPv4 地址不能直接在 IPv6 中使用，但 IPv6 支持一种特殊类别的地址：IPv4 映射地址。在 IPv4 映射地址中，前 80 位设置为零，接下来的 16 位设置为 1，最后 32 位表示 IPv4 地址。

    例如，这里是相同的 IP 地址以两种格式表示：

    ```java
    IPv4 address                  IPv4-mapped address (for use in IPv6)
    192.0.2.1                   ::ffff:192.0.2.1

    ```

    默认情况下，如果 IB 适配器中启用了 IPv6，则 Java 平台将使用 IPv6。然而，在 Solaris 操作系统或 Linux 下目前不支持 IPv4 映射地址。因此，如果您想在 JDK 7 下使用 IPv4 地址格式，必须指定`java.net.preferIPv4Stack`属性，如下例所示：

    ```java
    % java -Dcom.sun.sdp.conf=sdp.conf -Djava.net.preferIPv4Stack=true  *MyApplication*

    ```

+   错误

    在早期 InfiniBand 实现中发现了一些错误。这些错误在 Solaris 10 10/09 版本中已修复。请确保您至少使用此版本。
