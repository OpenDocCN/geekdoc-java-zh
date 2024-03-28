# 安全性

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/security.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/security.html)

LDAP 服务提供了一个通用的目录服务。它可以用来存储各种信息。所有 LDAP 服务器都有一套系统来控制谁可以读取和更新目录中的信息。

要访问 LDAP 服务，LDAP 客户端首先必须向服务*进行身份验证*。也就是说，它必须告诉 LDAP 服务器谁将访问数据，以便服务器可以决定客户端被允许看到和执行什么操作。如果客户端成功向 LDAP 服务器进行身份验证，那么当服务器随后收到来自客户端的请求时，它将检查客户端是否被允许执行该请求。这个过程称为*访问控制*。

LDAP 标准提出了 LDAP 客户端可以向 LDAP 服务器进行身份验证的方式（[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)和[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)）。这些内容在 LDAP 身份验证部分和身份验证机制部分中进行了概述。本课程还包含了如何使用匿名、简单和 SASL 身份验证机制的描述。

不同的 LDAP 服务器实现以不同的方式支持访问控制。本课程不讨论这个问题。

LDAP 服务的另一个安全方面是支持使用安全通道与客户端通信，例如发送和接收包含密码和密钥等秘密信息的属性。LDAP 服务器为此目的使用 SSL。本课程还展示了如何与 LDAP 服务提供者一起使用 SSL。
