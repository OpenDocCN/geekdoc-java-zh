# 什么是 URL？

> 原文：[`docs.oracle.com/javase/tutorial/networking/urls/definition.html`](https://docs.oracle.com/javase/tutorial/networking/urls/definition.html)

如果您一直在浏览网络，您无疑听说过 URL 这个术语，并且已经使用 URL 从网络访问 HTML 页面。

尽管不完全准确，但通常最容易将 URL 视为全球网络上文件的名称，因为大多数 URL 指向网络上某台机器上的文件。但请记住，URL 也可以指向网络上的其他资源，例如数据库查询和命令输出。

* * *

**定义：**

URL 是*统一资源定位符*的缩写，是指向互联网上资源的引用（地址）。

* * *

URL 有两个主要组成部分：

+   协议标识符：对于 URL `http://example.com`，协议标识符是 `http`。

+   资源名称：对于 URL `http://example.com`，资源名称是 `example.com`。

请注意，协议标识符和资源名称之间用冒号和两个正斜杠分隔。协议标识符指示要用于获取资源的协议的名称。此示例使用超文本传输协议（HTTP），通常用于提供超文本文档。HTTP 只是用于访问网络上不同类型资源的许多不同协议之一。其他协议包括文件传输协议（FTP）、Gopher、文件和新闻。

资源名称是资源的完整地址。资源名称的格式完全取决于所使用的协议，但对于许多协议，包括 HTTP，资源名称包含以下一个或多个组件：

**主机名**

存储资源的机器名称。

**文件名**

机器上文件的路径名。

**端口号**

要连接的端口号（通常是可选的）。

**参考**

指向资源内命名锚点的参考，通常标识文件内的特定位置（通常是可选的）。

对于许多协议，主机名和文件名是必需的，而端口号和参考是可选的。例如，HTTP URL 的资源名称必须指定网络上的服务器（主机名）和该机器上文档的路径（文件名）；它还可以指定端口号和参考。
