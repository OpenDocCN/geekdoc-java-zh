# LDAP v3

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/ldap.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/ldap.html)

## 国际化

国际化是通过国际字符集（ISO 10646）来处理的，用于表示协议元素是字符串（如 DN）。版本 3 还不同于版本 2，它使用 UTF-8 来编码其字符串。

## 认证

除了匿名、简单（明文密码）认证外，LDAP v3 还使用简单认证和安全层（SASL）认证框架（[RFC 2222](http://www.ietf.org/rfc/rfc2222.txt)）允许使用不同的认证机制与 LDAP 一起使用。SASL 指定了一种挑战-响应协议，其中数据在客户端和服务器之间交换，用于认证目的。

目前定义了几种 SASL 机制：[DIGEST-MD5](http://www.ietf.org/rfc/rfc2831.txt), [CRAM-MD5](http://www.ietf.org/rfc/rfc2195.txt), [Anonymous](http://www.ietf.org/rfc/rfc2245.txt), [External](http://www.ietf.org/rfc/rfc2222.txt), [S/Key](http://www.ietf.org/rfc/rfc2222.txt), [GSSAPI](http://www.ietf.org/rfc/rfc2222.txt), 和 [Kerberos v4](http://www.ietf.org/rfc/rfc2222.txt)。LDAP v3 客户端可以使用任何这些 SASL 机制，前提是 LDAP v3 服务器支持它们。此外，可以在不必更改 LDAP 的情况下使用新的（尚未定义的）SASL 机制。

## 转发

*转发*是服务器发送回客户端的信息，指示请求的信息可以在另一个位置（可能在另一个服务器上）找到。在 LDAP v2 中，服务器应处理转发而不将其返回给客户端。这是因为处理转发可能非常复杂，因此会导致更复杂的客户端。随着服务器的构建和部署，发现转发很有用，但并不是所有服务器都支持服务器端转发处理。因此，找到了一种方法来改进协议以允许返回转发。这是通过将转发放置在“部分结果”错误响应的错误消息中来完成的。

LDAP v3 明确支持转发，并允许服务器直接将转发返回给客户端。本课程不涵盖转发，但您可以随时参考[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/index.html)来管理应用程序中的转发。

## 部署

诸如 LDAP 之类的常见协议对确保所有目录客户端和服务器“说同一种语言”非常有用。当在网络中部署许多不同的目录客户端应用程序和目录服务器时，所有这些实体讨论相同的对象也非常有用。

*目录模式*指定了目录可能具有的对象类型以及每种对象类型可能具有的强制和可选属性，等等。LDAP v3 基于 X.500 标准为网络中常见的对象（如国家、地点、组织、用户/人员、组和设备）定义了一个模式（[RFC 2252](http://www.ietf.org/rfc/rfc2252.txt)和[RFC 2256](http://www.ietf.org/rfc/rfc2256.txt)）。它还定义了客户端应用程序访问服务器模式的方法，以便了解特定服务器支持的对象和属性类型。

LDAP v3 进一步定义了一组用于表示属性值的语法（[RFC 2252](http://www.ietf.org/rfc/rfc2252.txt)）。编写需要访问模式的 Java 应用程序，请参考[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/schema/index.html)。

## 扩展

除了预定义的操作集合，如“搜索”和“修改”，LDAP v3 还定义了一个*“扩展”操作*。 “扩展”操作以请求作为参数并返回响应。请求包含标识请求的标识符和请求的参数。响应包含执行请求的结果。 “扩展”操作请求/响应对称为*扩展*。例如，可以有一个用于启动 TLS 的扩展，这是客户端向服务器发出的激活启动 TLS 协议的请求。

这些扩展可以是标准的（由 LDAP 社区定义）或专有的（由特定目录供应商定义）。编写需要使用扩展的应用程序，请参考[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/index.html)。

## 控制

另一种添加新功能的方法是使用*控制*。LDAP v3 允许通过使用控制来修改任何操作的行为。可以在操作中发送任意数量的控制，并且可以在其结果中返回任意数量的控制。例如，您可以在“搜索”操作中发送一个排序控制，告诉服务器根据``"name"``属性对搜索结果进行排序。

像扩展一样，这些控制可以是标准的或专有的。标准控制在[平台](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Control.html)中提供。编写需要使用控制的应用程序，请参考[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/index.html)。
