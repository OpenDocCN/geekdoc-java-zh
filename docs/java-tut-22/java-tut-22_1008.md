# SASL

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/sasl.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/sasl.html)

LDAP v3 协议使用[SASL](http://www.ietf.org/rfc/rfc2222.txt)来支持*可插拔*身份验证。这意味着 LDAP 客户端和服务器可以根据客户端和服务器所需的保护级别协商和使用可能是非标准和/或定制的身份验证机制。LDAP v2 协议不支持 SASL。

目前定义了几种 SASL 机制：

+   匿名（[RFC 2245](http://www.ietf.org/rfc/rfc2245.txt)）

+   CRAM-MD5（[RFC 2195](http://www.ietf.org/rfc/rfc2195.txt)）

+   Digest-MD5（[RFC 2831](http://www.ietf.org/rfc/rfc2831.txt)）

+   外部（[RFC 2222](http://www.ietf.org/rfc/rfc2830.txt))

+   Kerberos V4（[RFC 2222](http://www.ietf.org/rfc/rfc2830.txt)）

+   Kerberos V5（[RFC 2222](http://www.ietf.org/rfc/rfc2830.txt)）

+   SecurID（[RFC 2808](http://www.ietf.org/rfc/rfc2808.txt)）

+   S/Key（[RFC 2222](http://www.ietf.org/rfc/rfc2830.txt)）

## LDAP 服务器支持的 SASL 机制

在上述列表中，流行的 LDAP 服务器（如 Oracle、OpenLDAP 和 Microsoft）支持外部、摘要-MD5 和 Kerberos V5。[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)提议将摘要-MD5 用作 LDAP v3 服务器的强制默认机制。

这是一个`简单的程序`，用于查找 LDAP 服务器支持的 SASL 机制列表。

```java
// Create initial context
DirContext ctx = new InitialDirContext();

// Read supportedSASLMechanisms from root DSE
Attributes attrs = ctx.getAttributes(
    "ldap://localhost:389", new String[]{"supportedSASLMechanisms"});

```

运行此程序针对支持外部 SASL 机制的服务器产生的输出如下。

```java
{supportedsaslmechanisms=supportedSASLMechanisms: 
                         EXTERNAL, GSSAPI, DIGEST-MD5}

```

## 指定身份验证机制

要使用特定的 SASL 机制，您需要在[`Context.SECURITY_AUTHENTICATION`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION)环境属性中指定其 IANA 注册的机制名称。您还可以指定 LDAP 提供程序尝试的机制列表。通过指定一个有序的以空格分隔的机制名称列表来实现。LDAP 提供程序将使用它找到实现的第一个机制。

这是一个示例，要求 LDAP 提供程序尝试获取 DIGEST-MD5 机制的实现，如果不可用，则使用 GSSAPI 的实现。

```java
env.put(Context.SECURITY_AUTHENTICATION, "DIGEST-MD5 GSSAPI");

```

您可以从应用程序的用户获取此身份验证机制列表。或者您可以通过类似于之前显示的调用询问 LDAP 服务器获取它。LDAP 提供程序本身不会向服务器查询此信息。它只是尝试定位和使用指定机制的实现。

平台中的 LDAP 提供程序内置支持外部、摘要-MD5 和 GSSAPI（Kerberos v5）SASL 机制。您可以添加对其他机制的支持。

## 指定身份验证机制的输入

一些机制，如 External，不需要额外的输入，仅凭借机制名称就足以进行认证。External 示例展示了如何使用 External SASL 机制。

大多数其他机制需要一些额外的输入。根据机制的不同，输入的类型可能会有所不同。以下是一些机制常见的输入要求。

+   **认证 ID**。执行认证的实体的身份。

+   **授权 ID**。如果认证成功，应该进行访问控制检查的实体的身份。

+   **认证凭据**。例如，密码或密钥。

认证和授权 ID 可能会有所不同，如果程序（如代理服务器）代表另一个实体进行认证。认证 ID 是通过使用[`Context.SECURITY_PRINCIPAL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL)环境属性指定的。它的类型是`java.lang.String`。

认证 ID 的密码/密钥是通过使用[`Context.SECURITY_CREDENTIALS`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_CREDENTIALS)环境属性指定的。它的类型是`java.lang.String`、`char` 数组（`char[]`）或`byte` 数组（`byte[]`）。如果密码是`byte`数组，则会使用 UTF-8 编码将其转换为`char`数组。

如果已设置`"java.naming.security.sasl.authorizationId"`属性，则其值将用作授权 ID。其值必须是`java.lang.String`类型。默认情况下，空字符串将用作授权 ID，这将指示服务器从客户端的认证凭据中派生授权 ID。

Digest-MD5 示例展示了如何使用`Context.SECURITY_PRINCIPAL`和`Context.SECURITY_CREDENTIALS`属性进行 Digest-MD5 认证。

如果某个机制需要除了已经描述的之外的输入，那么你需要为该机制定义一个*回调*对象供其使用，你可以在[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/callback.html)中查看回调示例。本课程的下一部分将讨论如何使用 SASL Digest-MD5 认证机制。[SASL 策略](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/sasl.html)、[GSS API（Kerberos v5）](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/gssapi.html) 和 [CRAM-MD5](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/crammd5.html) 机制在 JNDI 教程中有介绍。
