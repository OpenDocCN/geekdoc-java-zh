# Solaris 和 Linux 支持

> 原文：[`docs.oracle.com/javase/tutorial/sdp/sockets/support.html`](https://docs.oracle.com/javase/tutorial/sdp/sockets/support.html)

## Solaris 10

要测试 SDP 是否已启用，请使用`sdpadm`(1M)命令：

```java
% /usr/sbin/sdpadm status
SDP is Enabled

```

你可以使用 grep 命令在`/etc/path_to_inst`文件中搜索字符串"ibd"，以查看网络上支持的 IB 适配器列表。

其他你可能会发现有用的命令包括`ib`(7D)、`ibd`(7D)和`sdp`(7D)。

## Solaris 11

你可以使用以下命令获取 InfiniBand 分区链接信息：

```java
% dladm show-port -o LINK

```

其他你可能会发现有用的命令包括`ib`(7D)、`ibd`(7D)和`sdp`(7D)。

## Linux

你可以使用以下命令获取 InfiniBand 适配器的列表：

```java
% egrep "^[ \t]+ib" /proc/net/dev

```
