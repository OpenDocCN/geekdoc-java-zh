# Digest-MD5

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/digest.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/digest.html)

*Digest-MD5 认证*是 LDAP v3 服务器所需的认证机制（[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)）。因为 SASL 的使用是 LDAP v3 的一部分（[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)），仅支持 LDAP v2 的服务器不支持 Digest-MD5。

Digest-MD5 机制在 [RFC 2831](http://www.ietf.org/rfc/rfc2831.txt) 中有描述。它基于 HTTP 摘要认证（[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)）。在 Digest-MD5 中，LDAP 服务器发送包含各种认证选项以及一个特殊令牌给 LDAP 客户端的数据。客户端通过发送加密响应来回应，该响应指示其选择的认证选项。响应被加密以证明客户端知道其密码。LDAP 服务器然后解密并验证客户端的响应。

要使用 Digest-MD5 认证机制，必须设置认证环境属性如下。

[`Context.SECURITY_AUTHENTICATION`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION)。

设置为字符串`"DIGEST-MD5"`。

[`Context.SECURITY_PRINCIPAL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL)。

设置为主体名称。这是一个特定于服务器的格式。一些服务器支持登录用户 id 格式，例如 UNIX 或 Windows 登录屏幕中定义的格式。其他服务器接受专有名称。还有一些使用 [RFC 2829](http://www.ietf.org/rfc/rfc2829.txt) 中定义的授权 id 格式。在该 RFC 中，名称应为字符串`"dn:"`，后跟被认证实体的完全限定 DN，或者字符串`"u:"`，后跟用户 id。一些服务器接受多种格式。一些格式的示例是`"cuser"`，`"dn: cn=C. User, ou=NewHires, o=JNDITutorial"`，和`"u: cuser"`。此属性的数据类型必须是`java.lang.String`。

[`Context.SECURITY_CREDENTIALS`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_CREDENTIALS)。

设置为主体的密码（例如，`"mysecret"`）。它的类型可以是`java.lang.String`，`char` 数组（`char[]`），或`byte` 数组（`byte[]`）。如果密码是`java.lang.String`或`char[]`，则使用 UTF-8 进行编码以传输到服务器。如果密码是`byte[]`，则原样传输到服务器。

`以下示例`展示了客户端如何使用 Digest-MD5 对 LDAP 服务器进行认证。

```java
// Set up the environment for creating the initial context
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Authenticate as C. User and password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "DIGEST-MD5");
env.put(Context.SECURITY_PRINCIPAL, 
        "dn:cn=C. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

* * *

**注意：** [Oracle Directory Server, v5.2](http://www.oracle.com/technetwork/testcontent/index-085178.html) 支持具有明文密码的用户的 Digest-MD5 认证机制。您必须在创建用户之前设置密码加密模式。如果您已经创建了用户，请删除并重新创建。要使用管理控制台设置密码加密模式，请选择“配置”选项卡和“数据”节点。在“密码”窗格中，选择“无加密（CLEAR）”选项以进行“密码加密”。服务器接受简单的用户名（即具有一个条目的`"uid"`属性的值）和用户名称的“dn:”格式。有关详细信息，请参阅服务器的文档。

* * *

## 指定领域

*领域* 定义了选择身份验证实体（`Context.SECURITY_PRINCIPAL` 属性的值）的命名空间。服务器可能有多个领域。例如，大学的服务器可能配置为具有两个领域，一个用于学生用户，另一个用于教师用户。领域配置由目录管理员完成。一些目录具有默认的单个领域。例如，Oracle Directory Server, v5.2，使用机器的完全限定主机名作为默认领域。

在 Digest-MD5 认证中，您必须对特定领域进行身份验证。您可以使用以下身份验证环境属性来指定领域。如果您不指定领域，则服务器提供的任何一个领域都将被使用。

`java.naming.security.sasl.realm`

设置为主体的领域。这是一个部署特定和/或服务器特定的区分大小写的字符串。它标识应选择主体名称（`Context.SECURITY_PRINCIPAL`）的领域或域。如果此领域与服务器提供的领域之一不匹配，则身份验证失败。

`以下示例` 显示了如何设置环境属性以使用 Digest-MD5 进行身份验证并指定领域。要使此示例在您的环境中起作用，您必须更改源代码，以便领域值反映在您的目录服务器上的配置。

```java
// Authenticate as C. User and password "mysecret" in realm "JNDITutorial"
env.put(Context.SECURITY_AUTHENTICATION, "DIGEST-MD5");
env.put(Context.SECURITY_PRINCIPAL, 
        "dn:cn=C. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");
env.put("java.naming.security.sasl.realm", "JNDITutorial");

```

如果您需要使用[隐私保护](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/digest.html)和其他 SASL 属性，请参阅 JNDI 教程。
