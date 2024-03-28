# SSL 和自定义套接字

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/ssl.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/ssl.html)

除了 SASL 身份验证外，大多数 LDAP 服务器允许通过 SSL 访问其服务。SSL 对于 LDAP v2 服务器特别有用，因为 v2 协议不支持 SASL 身份验证。

启用 SSL 的服务器通常以两种方式支持 SSL。在最基本的方式中，服务器支持 SSL 端口以及普通（未受保护）端口。服务器支持 SSL 的另一种方式是通过使用“启动 TLS 扩展”（[RFC 2830](http://www.ietf.org/rfc/rfc2830.txt)）。此选项仅适用于 LDAP v3 服务器，并在该部分中有详细描述。

## 使用 SSL 套接字属性

默认情况下，JDK 中的 LDAP 服务提供程序在与 LDAP 服务器通信时使用普通套接字。要求使用 SSL 套接字，请将[`Context.SECURITY_PROTOCOL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PROTOCOL)属性设置为`"ssl"`。

在`以下示例`中，LDAP 服务器在端口 636 上提供 SSL。要运行此程序，您必须在 LDAP 服务器上的端口 636 上启用 SSL。此过程通常由目录管理员执行。

* * *

**服务器要求：** LDAP 服务器必须设置为具有 X.509 SSL 服务器证书并启用 SSL。通常，您必须首先从证书颁发机构（CA）为服务器获取签名证书。然后，按照目录供应商的说明启用 SSL。不同的供应商有不同的工具来执行此操作。

对于[Oracle Directory Server, v5.2](http://www.oracle.com/technetwork/testcontent/index-085178.html)，请在管理控制台中使用“管理证书”工具生成证书签名请求（CSR）。将 CSR 提交给 CA 以获取 X.509 SSL 服务器证书。使用管理控制台，将证书添加到服务器的证书列表中。如果 CA 的证书尚未在服务器的受信任 CA 列表中，则也需安装 CA 的证书。通过在管理控制台中使用“配置”选项卡启用 SSL。在左窗格中选择服务器。在右窗格中选择“加密”选项卡。选中“为此服务器启用 SSL”和“使用此密码族：RSA”的复选框，确保您添加的服务器证书在证书列表中。

**客户端要求：** 您需要确保客户端信任您将要使用的 LDAP 服务器。您必须在 JRE 的受信任证书数据库中安装服务器的证书（或其 CA 的证书）。这里是一个例子。

```java
# cd *JAVA_HOME*/lib/security
# keytool -import -file server_cert.cer -keystore jssecacerts

```

有关如何使用安全工具的信息，请参阅安全指南。有关 JSSE 的信息，请参阅[Java 安全套接字扩展（JSSE）参考指南](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html)。

* * *

```java
// Set up the environment for creating the initial context
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:636/o=JNDITutorial");

// Specify SSL
env.put(Context.SECURITY_PROTOCOL, "ssl");

// Authenticate as S. User and password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires,o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

* * *

**注意：** 如果您使用 SSL 连接到未使用 SSL 的端口的服务器，则您的程序将挂起。同样，如果您使用普通套接字连接到服务器的 SSL 套接字，则您的应用程序将挂起。这是 SSL 协议的特性。

* * *

## 使用 LDAPS URL

通过使用[`Context.SECURITY_PROTOCOL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PROTOCOL)属性请求使用 SSL，您还可以通过使用 LDAPS URL 请求使用 SSL。LDAPS URL 类似于 LDAP URL，只是 URL 方案为 "ldaps" 而不是 "ldap"。它指定与 LDAP 服务器通信时使用 SSL。

在`以下示例`中，LDAP 服务器在端口 636 上提供 SSL。要运行此程序，您必须在 LDAP 服务器上的端口 636 上启用 SSL。

```java
// Set up the environment for creating the initial context
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");

// Specify LDAPS URL
env.put(Context.PROVIDER_URL, "ldaps://localhost:636/o=JNDITutorial");

// Authenticate as S. User and password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

LDAPS URL 可以在任何接受 LDAP URL 的地方使用。查看[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/url.html)了解有关 LDAP 和 LDAPS URL 的详细信息。

## 客户端身份验证：使用外部 SASL 机制的 SSL

SSL 在比 LDAP 更低的层次提供身份验证和其他安全服务。如果在 SSL 上已经完成了身份验证，则 LDAP 层可以通过使用[External](http://www.ietf.org/rfc/rfc2222.txt) SASL 机制从 SSL 中使用该身份验证信息。

`以下示例`类似于`先前的 SSL 示例`，只是它不使用简单身份验证，而是使用外部 SASL 身份验证。通过使用 External，您无需提供任何主体或密码信息，因为它们会从 SSL 中获取。

* * *

**服务器要求：** 此示例要求 LDAP 服务器允许基于证书的客户端身份验证。此外，LDAP 服务器必须信任（CA 的）接收到的客户端证书，并且必须能够将客户端证书中的所有者可分辨名称映射到其了解的主体。请按照您的目录供应商的说明执行这些任务。

**客户端要求：** 此示例要求客户端具有 X.509 SSL 客户端证书。此外，证书必须存储为密钥库文件中的第一个密钥条目。如果此条目受密码保护，则必须与密钥库具有相同的密码。有关 JSSE 密钥库的更多信息，请参阅[Java 安全套接字扩展（JSSE）参考指南](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html)。

* * *

```java
// Set up the environment for creating the initial context
Hashtable<String, Object> env = new Hashtable<String, Object>(11);
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:636/o=JNDITutorial");

// Principal and credentials will be obtained from the connection
env.put(Context.SECURITY_AUTHENTICATION, "EXTERNAL");

// Specify SSL
env.put(Context.SECURITY_PROTOCOL, "ssl");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

...

```

要运行此程序以便使用客户端证书进行身份验证，您必须提供（作为系统属性）包含客户端证书的密钥库的位置和密码。以下是运行程序的示例。

```java
java -Djavax.net.ssl.keyStore=MyKeystoreFile \
    -Djavax.net.ssl.keyStorePassword=mysecret \
    External

```

如果您没有提供密钥库，程序将使用匿名身份验证运行，因为在 SSL 上不存在客户端凭据。

此示例展示了实现基于证书的客户端身份验证的最基本方法。 通过编写和使用访问客户端证书的自定义套接字工厂，可以以更灵活的方式实现更高级的方法，也许通过使用 LDAP 目录。 下一节将展示如何在 JNDI 应用程序中使用自定义套接字工厂。

## 使用自定义套接字

当使用 SSL 时，默认情况下，LDAP 提供程序将使用套接字工厂，[`javax.net.ssl.SSLSocketFactory`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/SSLSocketFactory.html)，用于创建与服务器通信的 SSL 套接字，使用默认的 JSSE 配置。 JSSE 可以以多种方式进行自定义，详细信息请参阅[Java 安全套接字扩展（JSSE）参考指南](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html)。 但是，有时这些自定义不足以满足需求，您需要对 LDAP 服务提供程序使用的 SSL 套接字或一般套接字进行更多控制。 例如，您可能需要能够绕过防火墙的套接字，或者 JSSE 套接字使用非默认的缓存/检索策略来管理其信任和密钥存储。 要设置 LDAP 服务提供程序使用的套接字工厂实现，请将`"java.naming.ldap.factory.socket"`属性设置为套接字工厂的完全限定类名。 此类必须实现[`javax.net.SocketFactory`](https://docs.oracle.com/javase/8/docs/api/javax/net/SocketFactory.html)抽象类，并提供实现[`getDefault()`](https://docs.oracle.com/javase/8/docs/api/javax/net/SocketFactory.html#getDefault--)方法的实例，该方法返回套接字工厂的实例。 请参阅[Java 安全套接字扩展（JSSE）参考指南](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html)。

这里是一个产生普通套接字的`自定义套接字工厂`的示例。

```java
public class CustomSocketFactory extends SocketFactory {
    public static SocketFactory getDefault() {

        System.out.println("[acquiring the default socket factory]");
        return new CustomSocketFactory();
    }
        ...
}

```

请注意，此示例每次创建新的 LDAP 连接时都会创建`CustomSocketFactory`的新实例。 这对某些应用程序和套接字工厂可能是合适的。 如果要重用相同的套接字工厂，`getDefault()`应返回一个单例。

要在 JNDI 程序中使用此自定义套接字工厂，请设置`"java.naming.ldap.factory.socket"`属性，如`以下示例`所示。

```java
// Set up the environment for creating the initial context
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Specify the socket factory
env.put("java.naming.ldap.factory.socket", "CustomSocketFactory");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

`"java.naming.ldap.factory.socket"` 属性可用于在每个上下文基础上设置套接字工厂。另一种控制 LDAP 服务提供程序使用的套接字的方法是通过使用[`java.net.Socket.setSocketImplFactory()`](https://docs.oracle.com/javase/8/docs/api/java/net/Socket.html#setSocketImplFactory-java.net.SocketImplFactory-)为整个程序中使用的所有套接字设置套接字工厂。使用这种方法不够灵活，因为它影响所有套接字连接，而不仅仅是 LDAP 连接，因此应谨慎使用。
