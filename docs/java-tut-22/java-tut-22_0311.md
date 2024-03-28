# Java 网络启动协议

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/jnlp.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/jnlp.html)

Java 网络启动协议（JNLP）使应用程序能够通过使用托管在远程 Web 服务器上的资源在客户端桌面上启动。Java 插件软件和 Java Web 启动软件被视为 JNLP 客户端，因为它们可以在客户端桌面上启动远程托管的小程序和应用程序。有关详细信息，请参阅[Java 网络启动协议和 API 规范更改日志](http://www.oracle.com/technetwork/java/javase/jnlp-spec-log-139509.html)。

最近部署技术的改进使我们能够通过使用 JNLP 启动丰富的互联网应用程序（RIA）。通过使用这种协议，可以启动小程序和 Java Web 启动应用程序。通过使用 JNLP 启动的 RIA 还可以访问 JNLP API。这些 JNLP API 允许 RIA 在用户许可的情况下访问客户端桌面。

JNLP 由 RIA 的 JNLP 文件启用。JNLP 文件描述了 RIA。JNLP 文件指定了主 JAR 文件的名称，运行 RIA 所需的 Java 运行时环境软件的版本，名称和显示信息，可选包，运行时参数，系统属性等。

您可以在以下主题中找到有关通过使用 JNLP 部署 RIA 的更多信息：

+   部署小程序

+   部署 Java Web 启动应用程序

+   JNLP API

+   JNLP 文件的结构
