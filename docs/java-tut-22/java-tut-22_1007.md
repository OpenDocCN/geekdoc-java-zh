# 简单

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/simple.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/simple.html)

*简单*身份验证包括向 LDAP 服务器发送客户端（用户）的完全限定 DN 和客户端的明文密码（参见[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)和[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)）。这种机制存在安全问题，因为密码可以从网络中读取。为了避免以这种方式暴露密码，您可以在加密通道（如 SSL）中使用简单身份验证机制，前提是 LDAP 服务器支持。

LDAP v2 和 v3 都支持简单身份验证。

要使用简单身份验证机制，必须设置三个身份验证环境属性如下。

[`Context.SECURITY_AUTHENTICATION`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION)。

设置为`"simple"`。

[`Context.SECURITY_PRINCIPAL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL)。

设置为正在进行身份验证的实体的完全限定 DN（例如，`"cn=S. User, ou=NewHires, o=JNDITutorial"`）。它的类型为`java.lang.String`。

[`Context.SECURITY_CREDENTIALS`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_CREDENTIALS)。

设置为主体的密码（例如，`"mysecret"`）。它的类型为`java.lang.String`，`char`数组（`char[]`）或`byte`数组（`byte[]`）。如果密码是`java.lang.String`或`char`数组，则在传输到服务器时使用 UTF-8 进行编码以供 LDAP v3 使用，使用 ISO-Latin-1 供 LDAP v2 使用。如果密码是`byte[]`，则按原样传输到服务器。

查看本节中早期的示例，演示如何使用简单身份验证。

* * *

**注意：** 如果您向`Context.SECURITY_CREDENTIALS`环境属性提供空字符串、空的`byte`/`char`数组或`null`，则身份验证机制将是`"none"`。这是因为 LDAP 要求简单身份验证的密码不能为空。如果未提供密码，则协议会自动将身份验证转换为`"none"`。
