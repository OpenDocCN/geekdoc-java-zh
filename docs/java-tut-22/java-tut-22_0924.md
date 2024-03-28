# 教程：使用 URL

> 原文：[`docs.oracle.com/javase/tutorial/networking/urls/index.html`](https://docs.oracle.com/javase/tutorial/networking/urls/index.html)

URL 是统一资源定位符的缩写。它是指向互联网上资源的引用（地址）。您向您喜爱的 Web 浏览器提供 URL，以便它可以定位互联网上的文件，就像您向邮局提供地址以便它可以找到您的通信对象一样。

与互联网交互的 Java 程序还可以使用 URL 来找到他们希望访问的互联网资源。Java 程序可以使用`java.net`包中的一个名为[`URL`](https://docs.oracle.com/javase/8/docs/api/java/net/URL.html)的类来表示 URL 地址。

* * *

**术语说明：**

术语*URL*可能会产生歧义。它可以指互联网地址或 Java 程序中的`URL`对象。在需要明确 URL 含义的情况下，本文使用"URL 地址"来指代互联网地址，使用"`URL`对象"来指代程序中`URL`类的实例。

* * *

## 什么是 URL？

URL 采用描述如何在互联网上找到资源的字符串形式。URL 有两个主要组成部分：访问资源所需的协议和资源的位置。

## 创建 URL

在您的 Java 程序中，您可以创建一个表示 URL 地址的 URL 对象。URL 对象始终指向绝对 URL，但可以从绝对 URL、相对 URL 或 URL 组件构造。

## 解析 URL

过去需要解析 URL 以查找主机名、文件名和其他信息的日子已经一去不复返了。有了有效的 URL 对象，您可以调用其任何访问器方法，从 URL 中获取所有这些信息，而无需进行任何字符串解析！

## 直接从 URL 读取

本节展示了您的 Java 程序如何使用`openStream()`方法从 URL 中读取信息。

## 连接到 URL

如果您想要的不仅仅是从 URL 中读取信息，您可以通过在 URL 上调用`openConnection()`来连接到它。`openConnection()`方法返回一个 URLConnection 对象，您可以用它进行更一般的与 URL 通信，如从中读取、向其中写入或查询其内容和其他信息。

## 从 URLConnection 读取和写入

一些 URL（例如许多与 cgi-bin 脚本相关的 URL）允许您（甚至要求您）向 URL 写入信息。例如，搜索脚本可能要求在执行搜索之前向 URL 写入详细的查询数据。本节展示了如何向 URL 写入信息以及如何获取结果。
