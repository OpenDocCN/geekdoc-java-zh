# 教程：部署自包含应用程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/index.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/index.html)

自包含应用程序由一个单独的可安装捆绑包组成，其中包含您的应用程序和运行应用程序所需的 JRE 的副本。当安装应用程序时，它的行为与任何本地应用程序相同。为用户提供自包含应用程序可以避免在浏览器中运行应用程序时出现的安全问题。

您可以通过提供自己的图标来自定义自包含应用程序。可以设置文件关联，因此当用户打开您的应用程序可以处理的文件时，您的应用程序会自动启动。支持多个入口点，因此您可以在单个自包含应用程序捆绑包中提供一套应用程序。

可使用 Java 打包工具打包自包含应用程序。`javapackager`命令可以从命令行创建自包含应用程序的捆绑包。NetBeans 也可以用于创建自包含应用程序捆绑包。本课程描述了如何使用 Ant 任务创建这些捆绑包。

## 其他参考资料

有关自包含应用程序的更多信息，请参阅 Java 平台标准版部署指南中的[自包含应用程序打包](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html)。

有关 Java 打包的 Ant 任务的信息，请参阅[JavaFX Ant Tasks](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_tasks.html)，用于打包 Java SE 和 JavaFX 应用程序。

有关`javapackager`命令的信息，请参阅[Java 部署工具](https://docs.oracle.com/javase/8/docs/technotes/tools/index.html#deployment)。
