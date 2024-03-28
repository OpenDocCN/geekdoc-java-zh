# 小程序可以和不能做什么

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/security.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/security.html)

当用户访问包含小程序的页面时，Java 小程序将加载到客户端。Java 小程序背后的安全模型旨在保护用户免受恶意小程序的侵害。

小程序可以是沙箱小程序或特权小程序。沙箱小程序在仅允许一组安全操作的安全沙箱中运行。特权小程序可以在安全沙箱之外运行，并具有广泛的访问客户端的能力。

未签名的小程序受限于安全沙箱，仅在用户接受小程序时才运行。由认可的证书颁发机构签名的小程序可以在安全沙箱中运行，或者可以请求权限在安全沙箱之外运行。在任何情况下，用户必须接受小程序的安全证书，否则小程序将被阻止运行。

建议您使用 Java 网络启动协议（JNLP）启动您的小程序，以利用扩展功能并改善用户体验。有关逐步说明小程序部署的详细信息，请参阅部署小程序。

建议您将您的小程序部署到 Web 服务器上，即使是用于测试。要在本地运行小程序，请将小程序添加到例外站点列表中，该列表可以从 Java 控制面板的安全选项卡中管理。

在本主题中，我们将讨论小程序的安全限制和功能。

## 沙箱小程序

沙箱小程序受限于安全沙箱，*可以*执行以下操作：

+   它们可以与它们来自的主机和端口建立网络连接。协议必须匹配，如果使用域名加载小程序，则必须使用域名连接回主机，而不是 IP 地址。

+   使用`java.applet.AppletContext`类的`showDocument`方法，它们可以轻松显示 HTML 文档。

+   它们可以调用同一页面上其他小程序的公共方法。

+   从本地文件系统（用户的`CLASSPATH`中的目录）加载的小程序没有网络加载的小程序所具有的任何限制。

+   它们可以读取安全系统属性。有关安全系统属性列表，请参阅 System Properties。

+   使用 JNLP 启动时，沙箱小程序还可以执行以下操作：

    +   它们可以在客户端打开、读取和保存文件。

    +   它们可以访问共享的系统剪贴板。

    +   它们可以访问打印功能。

    +   它们可以在客户端存储数据，决定如何下载和缓存小程序等。有关使用 JNLP API 开发小程序的更多信息，请参阅 JNLP API。

沙箱小程序*无法*执行以下操作：

+   他们无法访问客户端资源，如本地文件系统、可执行文件、系统剪贴板和打印机。

+   他们无法连接或从任何第三方服务器检索资源（任何不同于其来源服务器的服务器）。

+   他们无法加载本地库。

+   他们无法更改 SecurityManager。

+   他们无法创建一个 ClassLoader。

+   他们无法读取某些系统属性。请参见系统属性以获取被禁止的系统属性列表。

## 特权小程序

特权小程序不受强制执行在沙盒小程序上的安全限制，可以在安全沙箱之外运行。

* * *

**注意：** JavaScript 代码被视为未签名代码。当特权小程序从 HTML 页面中的 JavaScript 代码访问时，该小程序在安全沙箱内执行。这意味着特权小程序基本上表现得像一个沙盒小程序。

* * *

参见富互联网应用程序中的安全性以了解如何处理小程序。

## 附加信息

有关小程序安全性警告框的更多信息，请参见[探索安全警告功能](http://www.oracle.com/technetwork/articles/javase/appletwarning-135102.html)（oracle.com/technetwork 上的文章）