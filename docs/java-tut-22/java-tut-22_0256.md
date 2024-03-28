# 路径：部署

> 原文：[`docs.oracle.com/javase/tutorial/deployment/index.html`](https://docs.oracle.com/javase/tutorial/deployment/index.html)

Java 富互联网应用程序（RIA）是具有类似桌面应用程序特征的应用程序，但是通过互联网部署。Java RIA 可以作为 Java 小程序或 Java Web Start 应用程序开发和部署。

+   小程序 - Java 小程序在浏览器环境中运行。Java 插件软件控制 Java 小程序的执行和生命周期。

+   Java Web Start 应用程序 - Java Web Start 应用程序首次通过浏览器启动。随后可以通过桌面快捷方式启动。一旦下载了 Java Web Start 应用程序并且用户接受了其安全证书，它的行为几乎像一个独立的应用程序。

## 基于组件的架构用于富互联网应用程序

在过去，决定将 Java 富互联网应用程序作为小程序在浏览器内部部署，还是作为 Java Web Start 应用程序在浏览器外部部署，可能会显著影响应用程序的设计。有了最新的 Java 插件，这个决定变得简单多了。

传统上，应用程序在 `main` 方法中构建其用户界面，包括顶级 `Frame`。这种编程风格阻碍了应用程序在浏览器中的重新部署，因为它假定应用程序创建自己的 `Frame`。在浏览器中作为小程序运行时，小程序是应该容纳应用程序用户界面的顶级容器。不需要顶级 `Frame`。

在设计 Java 富互联网应用程序时，请使用*基于组件的架构*。尝试将其功能组织成一个或多个可以组合在一起的组件。在这种情况下，“组件”一词指的是 AWT `Component` 类的子类、Swing `JComponent` 类或另一个子类的 GUI 元素。例如，您可以有一个顶级 `JPanel`，其中包含其他 UI 组件（如更多嵌套的 JPanels 和文本字段、组合框等）。通过这种设计，将核心功能部署为小程序或 Java Web Start 应用程序变得相对容易。

要部署为 Java 小程序，只需将核心功能封装在 `Applet` 或 `JApplet` 中，并添加必要的浏览器特定功能。要部署为 Java Web Start 应用程序，请将功能封装在 `JFrame` 中。

## 选择 Java 小程序和 Java Web Start 应用程序之间

富互联网应用程序决策指南包含详细信息，帮助您决定将代码部署为 Java 小程序还是 Java Web Start 应用程序。

## 自包含应用程序替代方案

自包含应用程序提供了一种部署选项，不需要浏览器。用户在本地安装您的应用程序，并类似于本机应用程序运行它。自包含应用程序包括运行应用程序所需的 JRE，因此用户始终拥有正确的 JRE。

本教程讨论了 RIA 和自包含应用程序的开发和部署。请查看[新功能](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/whatsnew_deployment.html)，了解客户端 Java 运行时环境（JRE）软件各个版本引入的功能。

![trail icon **开发和部署 Java Applets**](img/index.html)

![trail icon **开发和部署 Java Web Start 应用程序**](img/index.html)

![trail icon **使用 Java 富互联网应用程序做更多事情**](img/index.html)

![trail icon **深入部署**](img/index.html)

![trail icon **部署自包含应用程序**](img/index.html)

支持工具

![trail icon **在 JAR 文件中打包程序**](img/index.html)
