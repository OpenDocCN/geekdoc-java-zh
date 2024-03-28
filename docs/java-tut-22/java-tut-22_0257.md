# 课程：Java 小程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/index.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/index.html)

本课程讨论了 Java 小程序的基础知识，如何开发与环境交互丰富的小程序，以及如何部署小程序。

Java 小程序是一种特殊类型的 Java 程序，可以由启用了 Java 技术的浏览器从互联网上下载并运行。小程序通常嵌入在网页中，并在浏览器的上下文中运行。小程序必须是 `java.applet.Applet` 类的子类。`Applet` 类提供了小程序与浏览器环境之间的标准接口。

Swing 提供了 `Applet` 类的一个特殊子类，名为 `javax.swing.JApplet`。应该使用 `JApplet` 类来构建使用 Swing 组件构建其图形用户界面（GUI）的所有小程序。

浏览器的 Java 插件软件管理小程序的生命周期。

使用 Web 服务器来测试本课程中的示例。不建议使用本地小程序，当 Java 控制面板中的安全级别设置为高或非常高时，本地小程序将被阻止。

* * *

**注意：** 要运行小程序，请在 Microsoft Edge 上使用 IE 模式。请参阅[Microsoft Edge + Internet Explorer mode: Getting Started guide](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWEHMs)。

* * *

* * *

**注意：** 请确保您的客户端机器上至少安装了[Java SE Development Kit (JDK) 6 update 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本，然后再继续。您将需要此版本才能查看示例丰富的互联网应用程序并在不中断的情况下阅读后续章节。

* * *
