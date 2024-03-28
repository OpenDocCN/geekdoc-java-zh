# 创建一个 SDP 配置文件

> 原文：[`docs.oracle.com/javase/tutorial/sdp/sockets/file.html`](https://docs.oracle.com/javase/tutorial/sdp/sockets/file.html)

一个 SDP 配置文件是一个文本文件，您可以决定这个文件将存放在文件系统的哪个位置。配置文件中的每一行都是一个注释或一个规则。注释以井号字符(#)开头，井号字符后的所有内容将被忽略。

有两种类型的规则，如下：

+   "bind"规则表示当 TCP 套接字绑定到与规则匹配的地址和端口时，应使用 SDP 协议传输。

+   "connect"规则表示当未绑定的 TCP 套接字尝试连接到与规则匹配的地址和端口时，应使用 SDP 协议传输。

一个规则的形式如下：

```java
("bind"|"connect")1*LWSP-char(hostname|ipaddress)["/"prefix])1*LWSP-char("*"|port)["-"("*"|port)]

```

* * *

**解码符号：**

*1*LWSP-char*表示任意数量的线性空白字符（制表符或空格）可以分隔标记。方括号表示可选文本。符号(*xxx* | *yyy*)表示标记将包括*xxx*或*yyy*中的一个，但不会同时包括两者。引号中的字符表示文字。

* * *

第一个关键字指示规则是**bind**还是**connect**规则。下一个标记指定主机名或文字 IP 地址。当指定文字 IP 地址时，您还可以指定前缀，表示 IP 地址范围。第三个也是最后一个标记是端口号或端口号范围。

在这个示例配置文件中考虑以下符号：

```java
# Use SDP when binding to 192.0.2.1
bind 192.0.2.1 *

# Use SDP when connecting to all application services on 192.0.2.*
connect 192.0.2.0/24     1024-*

# Use SDP when connecting to the http server or a database on examplecluster
connect examplecluster.example.com   80
connect examplecluster.example.com   3306

```

示例文件中的第一条规则指定 SDP 用于本地 IP 地址`192.0.2.1`上的任何端口(*)。对于每个分配给 InfiniBand 适配器的本地地址，您将添加一个绑定规则。（*InfiniBand 适配器*相当于 InfiniBand 的网络接口卡（NIC）。）如果您有多个 IB 适配器，您将为分配给这些适配器的每个地址使用一个绑定规则。

示例文件中的第二条规则指定每当连接到`192.0.2.*`并且目标端口大于或等于 1024 时，将使用 SDP。IP 地址前缀`/24`表示 32 位 IP 地址的前 24 位应与指定地址匹配。IP 地址的每个部分使用 8 位，因此 24 位表示 IP 地址应匹配`192.0.2`，最后一个字节可以是任何值。端口标记上的`-*`符号指定“及以上”。端口范围，例如 1024—2056，也是有效的，并且将包括指定范围的端点。

示例文件中的最后规则指定一个主机名（`examplecluster`），首先分配给 http 服务器的端口（80），然后分配给数据库的端口（3306）。与文字 IP 地址不同，主机名可以转换为多个地址。当指定主机名时，它匹配主机名在名称服务中注册的所有地址。
