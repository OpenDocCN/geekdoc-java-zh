# 课程：LDAP 用户的高级主题

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/index.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/index.html)

**LDAP**教程中的课程提供了 LDAP 和 JNDI 之间的映射细节。它们还提供了通过 JNDI 访问 LDAP 服务的提示和技巧。

## LDAP

X.500 是 CCITT 关于目录服务的标准，是 OSI 服务套件的一部分。X.500 标准定义了一个协议（其中之一）用于客户端应用程序访问 X.500 目录，称为*目录访问协议*（DAP）。它建立在开放系统互连（OSI）协议栈之上。

因特网社区意识到需要类似 X.500 的服务，但面临不同的底层网络基础设施（TCP/IP 而不是 OSI），设计了一种基于 X.500 DAP 协议的新协议，称为*轻量级* DAP，或 LDAP。[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)定义了现在称为*版本 3*的 LDAP（或 LDAP v3），这是其前身 LDAP v2 的改进版本，其规范在[RFC 1777](http://www.ietf.org/rfc/rfc1777.txt)中指定。

LDAP 的目标是设计一种易于实现的协议，特别关注能够构建小型和简单的客户端。它试图通过大量使用字符串和尽可能减少结构的使用来实现简化。例如，DN 在协议中表示为字符串，属性类型名称和许多属性值也是如此。

该协议由客户端向服务器发送请求，服务器做出响应，尽管不一定按照请求发送的顺序。每个请求都带有一个 ID 标记，以便匹配请求和响应。该协议可以在 TCP 或 UDP 上运行，尽管 TCP 版本最常用。

由于对客户端的关注，LDAP 社区还定义了有关 DN 的字符串表示（[RFC 2553](http://www.ietf.org/rfc/rfc2553.txt)）、搜索过滤器（[RFC 1960](http://www.ietf.org/rfc/rfc1960.txt)）和属性语法（[RFC 1778](http://www.ietf.org/rfc/rfc1778.txt)）的标准，以及基于 C 语言的 API（[RFC 1823](http://www.ietf.org/rfc/rfc1823.txt)），以及用于访问 LDAP 服务的 URL 格式（[RFC 1959](http://www.ietf.org/rfc/rfc1959.txt)）。

LDAP v3 支持国际化、各种认证机制、引荐和通用部署机制。它允许通过使用*扩展*和*控制*向协议添加新功能，而无需对协议进行更改。
