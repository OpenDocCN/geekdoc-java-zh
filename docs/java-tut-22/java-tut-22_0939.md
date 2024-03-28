# 课程：程序化访问网络参数

> 原文：[`docs.oracle.com/javase/tutorial/networking/nifs/index.html`](https://docs.oracle.com/javase/tutorial/networking/nifs/index.html)

系统通常会同时运行多个活动网络连接，比如有线以太网，`802.11 b/g`（无线）和蓝牙。一些应用程序可能需要访问这些信息，以便在特定连接上执行特定的网络活动。

[`java.net.NetworkInterface`](https://docs.oracle.com/javase/8/docs/api/java/net/NetworkInterface.html) 类提供了访问这些信息的途径。

本课程将指导您了解此类的一些常见用法，并提供列出机器上所有网络接口及其 IP 地址和状态的示例。

## 什么是网络接口？

本页面描述了一个网络接口，并解释了为什么您可能想要使用它。

## 检索网络接口

本页面包含一个示例，演示了客户端程序如何检索机器上的所有网络接口。

## 列出网络接口地址

本页面向您展示如何列出分配给机器上所有网络接口的 IP 地址。

## 网络接口参数

本页面向您展示如何确定网络接口是否正在运行，或者网络接口是环回接口、点对点接口还是虚拟接口。您还可以了解如何确定接口是否支持多播。
