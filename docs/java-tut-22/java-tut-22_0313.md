# 部署最佳实践

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/bestPractices.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/bestPractices.html)

你可以通过本主题中描述的最佳实践来提高富互联网应用程序（RIA）的用户体验。

+   使用来自认可证书颁发机构的证书对 RIA 进行签名。确保所有工件都已签名，并且证书尚未过期。请参阅签署和验证 JAR 文件以获取有关签名的信息。

+   请求所需的最低权限级别。如果 RIA 不需要对用户系统的无限制访问，请指定权限级别为沙盒。请参阅富互联网应用程序中的安全性以获取更多安全指南。

+   优化 JAR 文件和相关资源的大小，以便您的 RIA 可以快速加载。请参阅减少下载时间以获取优化技巧。

+   启用版本下载协议并使用后台更新检查以使您的 RIA 快速启动。请参阅避免不必要的更新检查以了解更多关于版本下载协议和更新检查的信息。

+   确保客户端具有所需版本的 Java 运行时环境软件。请参阅确保 JRE 软件的存在以了解部署工具包脚本如何用于此目的的详细信息。

+   将 applet 的 JNLP 文件内容嵌入`<applet>`标签中，以避免从网络加载 JNLP 文件。此功能是在 Java SE 7 版本中引入的。请参阅在 Applet 标签中嵌入 JNLP 文件以了解如何在网页中嵌入 applet 的 JNLP 文件内容。

+   如有可能，预加载您的 Java Web Start 应用程序。如果您计划将 RIA 部署为具有一定管理控制权的企业中的 Java Web Start 应用程序，则可以将应用程序预加载到各个客户端，以便缓存并准备使用。使用以下命令预加载您的 Java Web Start 应用程序：

    ```java
    javaws -import -silent *<jnlp url>*

    ```
