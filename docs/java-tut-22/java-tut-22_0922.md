# 你可能已经了解的关于 Java 网络的知识

> 原文：[`docs.oracle.com/javase/tutorial/networking/overview/alreadyknow.html`](https://docs.oracle.com/javase/tutorial/networking/overview/alreadyknow.html)

“网络”这个词让许多程序员心生恐惧。不要害怕！在 Java 环境中提供的网络功能非常容易使用。事实上，你可能已经在不知不觉中使用网络了！

## 从网络加载小程序

如果你有一个支持 Java 的浏览器，你肯定已经执行了许多小程序。你运行的小程序是通过 HTML 文件中的特殊标签引用的 - `<APPLET>`标签。小程序可以位于任何地方，无论是在本地计算机上还是在互联网上的某个地方。小程序的位置对用户完全不可见。然而，小程序的位置被编码在`<APPLET>`标签中。浏览器解码这些信息，找到小程序并运行它。如果小程序位于你自己的机器之外的某台机器上，浏览器必须在运行之前下载小程序。

这是你从 Java 开发环境访问互联网的最高级别。其他人已经花时间编写了一个浏览器，完成了连接到网络和从中获取数据的所有繁重工作，从而使你能够从世界各地运行小程序。

**更多信息：**

“Hello World!”应用程序展示了如何编写你的第一个小程序并运行它。

Java 小程序系列描述了如何从头到尾编写 Java 小程序。

## 从 URL 加载图像

如果你尝试编写自己的 Java 小程序和应用程序，你可能已经遇到了 java.net 包中的一个类叫做 URL。这个类代表统一资源定位符，是网络上某些资源的地址。你的小程序和应用程序可以使用 URL 来引用甚至连接到网络上的资源。例如，要从网络加载图像，你的 Java 程序必须首先创建一个包含图像地址的 URL。

这是你可以与互联网互动的下一个最高级别 - 你的 Java 程序获取它想要的东西的地址，为其创建一个 URL，然后使用 Java 开发环境中的某个现有函数来连接到网络并检索资源。

**更多信息：**

如何使用图标展示了当你有图像的 URL 时如何将其加载到你的 Java 程序中（无论是小程序还是应用程序）。在加载图像之前，你必须创建一个包含资源地址的 URL 对象。

处理 URL，这一系列中的下一课，提供了关于 URL 的完整讨论，包括你的程序如何连接到它们以及如何从连接中读取和写入数据。