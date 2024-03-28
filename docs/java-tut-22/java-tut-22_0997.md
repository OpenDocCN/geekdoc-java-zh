# 故障排除提示

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/faq.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/faq.html)

运行成功编译的使用 JNDI 类的程序时可能遇到的最常见问题如下。

* * *

1.  没有初始上下文

1.  连接被拒绝

1.  连接失败

1.  程序挂起

1.  找不到名称

1.  无法连接到任意主机

1.  无法访问系统属性进行配置

1.  无法使用 CRAM-MD5 进行身份验证

* * *

1\. 您收到了`NoInitialContextException`。

*原因*：您没有指定用于初始上下文的实现。具体来说，[`Context.INITIAL_CONTEXT_FACTORY`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#INITIAL_CONTEXT_FACTORY) 环境属性未设置为将创建初始上下文的工厂类名。或者，您没有使程序可用于服务提供者类（由`Context.INITIAL_CONTEXT_FACTORY`命名）。

*解决方案*：将`Context.INITIAL_CONTEXT_FACTORY`环境属性设置为您正在使用的初始上下文实现的类名。有关详细信息，请参阅配置部分。

如果已设置属性，请确保类名未拼写错误，并且所命名的类对您的程序可用（在其类路径中或安装在 JRE 的 `jre/lib/ext` 目录中）。Java 平台包括用于 LDAP、COS 命名、DNS 和 RMI 注册表的服务提供者。所有其他服务提供者必须安装并添加到执行环境中。

2\. 您收到了`CommunicationException`，指示“连接被拒绝”。

*原因*：由`Context.PROVIDER_URL`环境属性标识的服务器和端口未被服务器提供。也许有人已禁用或关闭了运行服务器的机器。或者，您可能拼写错误了服务器的名称或端口号。

*解决方案*：检查该端口上确实有服务器在运行，并在必要时重新启动服务器。您执行此检查的方式取决于您使用的 LDAP 服务器。通常，可用管理控制台或工具来管理服务器。您可以使用该工具验证服务器的状态。

3\. LDAP 服务器对其他实用程序（如其管理控制台）做出响应，但似乎没有对您程序的请求做出响应。

*原因*：服务器未对 LDAP v3 连接请求做出响应。一些服务器（特别是公共服务器）未正确响应 LDAP v3，而是忽略请求而不是拒绝它们。此外，一些 LDAP v3 服务器在处理 Oracle 的 LDAP 服务提供者自动发送的控件时存在问题，并经常返回服务器特定的失败代码。

*解决方案*。尝试将环境属性`"java.naming.ldap.version"`设置为`"2"`。LDAP 服务提供程序默认尝试使用 LDAP v3 连接到 LDAP 服务器；如果失败，则使用 LDAP v2。如果服务器悄悄忽略了 v3 请求，则提供程序将假定请求成功。为了解决这样的服务器问题，您必须显式设置协议版本以确保服务器的正确行为。

如果服务器是一个 v3 服务器，那么在创建初始上下文之前尝试设置以下环境属性：

```java
env.put(Context.REFERRAL, "throw");

```

这将关闭 LDAP 提供程序自动发送的控制。（查看[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/index.html)获取详细信息。）

4\. 程序挂起。

*原因*：一些服务器（特别是公共服务器）在尝试执行会生成太多结果或需要服务器检查太多条目才能生成答案的搜索时，不会响应（甚至不会给出负面答复）。这些服务器试图限制它们在每个请求基础上消耗的资源量。

或者，您尝试使用安全套接字层（SSL）与不支持它的服务器/端口进行通信，反之亦然（也就是说，您尝试使用普通套接字与 SSL 端口进行通信）。

最后，服务器要么由于负载过重而响应非常缓慢，要么由于某种原因根本不响应。

*解决方案*：如果您的程序因服务器试图限制其资源的使用而挂起，则重试您的请求，使用将返回单个结果或仅返回少量结果的查询。这将帮助您确定服务器是否存活。如果是，则可以扩大您的初始查询并重新提交。

如果您的程序因 SSL 问题而挂起，则需要找出端口是否为 SSL 端口，然后适当设置[`Context.SECURITY_PROTOCOL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PROTOCOL)环境属性。如果端口是 SSL 端口，则应将此属性设置为`"ssl"`。如果不是 SSL 端口，则不应设置此属性。

如果您的程序因为以上原因之一而挂起，属性`com.sun.jndi.ldap.read.timeout`就派上用场了，用于指定读取超时。该属性的值是表示 LDAP 操作的毫秒级读取超时的整数的字符串表示。如果 LDAP 提供程序在该时间段内无法获得 LDAP 响应，则会中止读取尝试。整数应大于零。小于或等于零的整数表示未指定读取超时，相当于一直等待响应直到收到。

如果未指定此属性，则默认是等待响应直到收到为止。

例如，

`env.put("com.sun.jndi.ldap.read.timeout", "5000");`会导致 LDAP 服务提供程序在服务器在 5 秒内没有回复时中止读取尝试。

5\. 您收到`NameNotFoundException`。

*原因*：当您为 LDAP 初始化初始上下文时，您提供了根专有名称。例如，如果您为初始上下文设置`Context.PROVIDER_URL`环境属性为`"ldap://ldapserver:389/o=JNDITutorial"`，然后提供了一个名称，如`"cn=Joe,c=us"`，那么您传递给 LDAP 服务的完整名称将是`"cn=Joe,c=us,o=JNDITutorial"`。如果这确实是您打算的名称，请检查您的服务器，确保它包含这样的条目。

此外，如果您为认证目的提供了不正确的专有名称，Oracle 目录服务器也会返回此错误。例如，如果您将[`Context.SECURITY_PRINCIPAL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL)环境属性设置为`"cn=Admin, o=Tutorial"`，而`"cn=Admin, o=Tutorial"`不是 LDAP 服务器上的条目，那么 LDAP 提供程序将抛出[`NameNotFoundException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameNotFoundException.html)。实际上，Oracle 目录服务器应返回与认证相关的错误，而不是“未找到名称”。

*解决方案*：验证您提供的名称是否是服务器上已存在的条目。您可以通过列出条目的父上下文或使用其他工具（如目录服务器的管理控制台）来实现这一点。

* * *

在尝试部署使用 JNDI 类的小程序时，您可能会遇到一些问题。

6\. 当您的小程序尝试与在与加载小程序的机器不同的机器上运行的目录服务器通信时，您会收到`AppletSecurityException`。

*原因*：您的小程序未经签名，因此只能连接到加载它的机器。或者，如果小程序已经签名，浏览器尚未授予小程序连接到目录服务器机器的权限。

*解决方案*：如果您希望允许小程序连接到运行在任意机器上的目录服务器，则需要对您的小程序和小程序将使用的所有 JNDI JAR 文件进行签名。有关签署 jar 文件的信息，请参见签署和验证 JAR 文件。

7\. 当您的小程序尝试使用系统属性设置环境属性时，您会收到`AppletSecurityException`。

*原因*：Web 浏览器限制对系统属性的访问，并且如果您尝试读取它们，则会抛出`SecurityException`。

*解决方案*：如果您需要为您的小程序获取输入，则尝试使用小程序参数。

8\. 当在 Firefox 中运行的小程序尝试使用 CRAM-MD5 对 LDAP 服务器进行身份验证时，您会收到`AppletSecurityException`。

*原因*: Firefox 禁用对 `java.security` 包的访问。LDAP 提供程序使用了 `java.security.MessageDigest` 提供的消息摘要功能来实现 CRAM-MD5。

*解决方案*: 使用 Java 插件。

* * *
