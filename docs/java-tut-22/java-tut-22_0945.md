# 使用 Cookies 进行 HTTP 状态管理

> 原文：[`docs.oracle.com/javase/tutorial/networking/cookies/definition.html`](https://docs.oracle.com/javase/tutorial/networking/cookies/definition.html)

HTTP 状态管理机制指定了一种通过 HTTP 请求和响应创建有状态会话的方式。

通常，HTTP 请求/响应对是相互独立的。然而，状态管理机制使得可以交换状态信息的客户端和服务器将这些对放入更大的上下文中，这被称为*会话*。用于创建和维护会话的状态信息被称为*cookie*。

一个 cookie 是可以存储在浏览器缓存中的数据片段。如果您访问一个网站然后再次访问它，cookie 数据可以用来识别您为回访者。Cookies 使得状态信息，比如在线购物车，可以被记住。一个 cookie 可以是短期的，保存数据一个网页会话，也就是直到您关闭浏览器，或者一个 cookie 可以是长期的，保存数据一周或一年。

关于 HTTP 状态管理的更多信息，请参阅[RFC 2965: HTTP 状态管理机制](http://www.ietf.org/rfc/rfc2965.txt)。
