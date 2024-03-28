# 身份验证机制

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/auth_mechs.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/auth_mechs.html)

不同版本的 LDAP 支持不同类型的身份验证。LDAP v2 定义了三种身份验证类型：匿名、简单（明文密码）和 Kerberos v4。

LDAP v3 支持匿名、简单和 SASL 身份验证。SASL 是简单身份验证和安全层（[RFC 2222](http://www.ietf.org/rfc/rfc2222.txt)）的缩写。它指定了一种挑战-响应协议，客户端和服务器之间交换数据以进行身份验证，并建立安全层以进行后续通信。通过使用 SASL，LDAP 可以支持 LDAP 客户端和服务器协商的任何类型的身份验证。

本课程包含了如何使用匿名、简单和 SASL 身份验证进行身份验证的描述。

## 指定身份验证机制

身份验证机制是通过使用[`Context.SECURITY_AUTHENTICATION`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION)环境属性来指定的。该属性可以具有以下值之一。

| 属性名称 | 属性值 |
| --- | --- |
| *sasl_mech* | 一个以空格分隔的 SASL 机制名称列表。使用列出的 SASL 机制之一（例如，`"CRAM-MD5"`表示使用[RFC 2195](http://www.ietf.org/rfc/rfc2195.txt)中描述的 CRAM-MD5 SASL 机制）。 |
| `none` | 不使用身份验证（匿名） |
| `simple` | 使用弱身份验证（明文密码） |

## 默认机制

如果客户端没有指定任何身份验证环境属性，则默认身份验证机制为`"none"`。然后客户端将被视为匿名客户端。

如果客户端在不显式指定`Context.SECURITY_AUTHENTICATION`属性的情况下指定身份验证信息，则默认身份验证机制为`"simple"`。
