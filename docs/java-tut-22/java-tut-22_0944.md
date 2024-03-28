# 课程：处理 Cookies

> 原文：[`docs.oracle.com/javase/tutorial/networking/cookies/index.html`](https://docs.oracle.com/javase/tutorial/networking/cookies/index.html)

虽然您可能已经熟悉 cookies，但您可能不知道如何在您的 Java 应用程序中充分利用它们。本课程将引导您了解 cookies 的概念，并解释如何设置 cookie 处理程序，以便您的 HTTP URL 连接将使用它。

Java SE 为此功能提供了一个主要类，[`java.net.CookieHandler`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieHandler.html)，以及以下支持类和接口：[`java.net.CookieManager`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html)，[`java.net.CookiePolicy`](https://docs.oracle.com/javase/8/docs/api/java/net/CookiePolicy.html)，[`java.net.CookieStore`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieStore.html)，和 [`java.net.HttpCookie`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpCookie.html)。

## 使用 Cookies 进行 HTTP 状态管理

本页面描述了 cookies 并解释了它们如何用于提供会话。

## CookieHandler 回调机制

当您访问网站时，本页面解释了 cookie 处理程序是如何被调用以及如何设置 cookie 处理程序。

## 默认 CookieManager

Java SE 提供了一个默认的 cookie 处理程序实现，在大多数情况下都足够使用，并且高度可定制。

## 自定义 CookieManager

这里有一些如何自定义 cookie 策略并编写自己的 cookie 存储的示例。
