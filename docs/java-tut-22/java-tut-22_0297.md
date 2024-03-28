# Cookie

> 原文：[`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/cookies.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/cookies.html)

Web 应用程序通常是一系列的超文本传输协议（HTTP）请求和响应。由于 HTTP 是一个无状态协议，信息不会在 HTTP 请求之间自动保存。Web 应用程序使用 cookie 在客户端上存储状态信息。Cookie 可用于存储有关用户、用户的购物车等信息。

## Cookie 的类型

两种类型的 cookie 如下：

+   会话 cookie – 会话 cookie 存储在内存中，只要用户在使用 Web 应用程序，就可以访问。用户退出 Web 应用程序时，会话 cookie 会丢失。这些 cookie 由会话 ID 标识，最常用于存储购物车的详细信息。

+   永久 cookie – 永久 cookie 用于存储长期信息，如用户偏好和用户识别信息。永久 cookie 存储在持久存储中，用户退出应用程序时不会丢失。永久 cookie 在过期时会丢失。

## 富互联网应用程序中的 Cookie 支持

富互联网应用程序（小程序和 Java Web Start 应用程序）支持会话和永久 cookie。底层的 cookie 存储取决于客户端的浏览器和操作系统。

要了解更多关于 cookie 的信息，请参阅以下内容：

+   在 Java 教程中的处理 Cookie 课程

+   [CookieManager 的 API 文档](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html)和相关类
