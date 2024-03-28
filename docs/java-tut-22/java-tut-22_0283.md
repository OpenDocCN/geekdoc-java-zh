# 教程：Java Web Start

> 原文：[`docs.oracle.com/javase/tutorial/deployment/webstart/index.html`](https://docs.oracle.com/javase/tutorial/deployment/webstart/index.html)

Java Web Start 软件提供了一键启动功能，可以启动功能齐全的应用程序。用户可以下载和启动应用程序，例如完整的电子表格程序或互联网聊天客户端，而无需经历冗长的安装过程。

使用 Java Web Start 软件，用户可以通过在网页中点击链接来启动 Java 应用程序。该链接指向一个 Java 网络启动协议（JNLP）文件，该文件指示 Java Web Start 软件下载、缓存和运行应用程序。

Java Web Start 软件为 Java 开发人员和用户提供了许多部署优势：

+   使用 Java Web Start 软件，您可以将单个 Java 应用程序放在 Web 服务器上，以便在各种平台上部署，包括 Windows、Linux 和 Solaris。

+   Java Web Start 软件支持多个同时运行的 Java 平台版本。应用程序可以请求特定版本的 Java 运行时环境（JRE）软件，而不会与其他应用程序的需求发生冲突。

+   用户可以创建桌面快捷方式以在浏览器外启动 Java Web Start 应用程序。

+   Java Web Start 软件利用了 Java 平台固有的安全性。默认情况下，应用程序对本地磁盘和网络资源的访问受到限制。

+   使用 Java Web Start 软件启动的应用程序会在本地缓存以提高性能。

+   Java Web Start 应用程序的更新会在用户从桌面独立运行应用程序时自动下载。

Java Web Start 软件作为 JRE 软件的一部分安装。用户无需单独安装 Java Web Start 软件或执行其他任务即可使用 Java Web Start 应用程序。

## 其他参考资料

本课程旨在帮助您开始使用 Java Web Start 技术，并不包括所有可用文档。有关 Java Web Start 技术的更多信息，请参阅以下内容：

+   [Java Web Start 指南](https://docs.oracle.com/javase/8/docs/technotes/guides/javaws/developersguide/contents.html)

+   [Java Web Start 常见问题解答](https://docs.oracle.com/javase/8/docs/technotes/guides/javaws/developersguide/faq.html)

+   [JNLP 规范](http://jcp.org/en/jsr/detail?id=56)

+   [`javax.jnlp` API 文档](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/index.html)

+   [Java Web Start 开发者站点](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)
