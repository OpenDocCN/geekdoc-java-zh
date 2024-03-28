# LDAP 操作如何映射到 JNDI API

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/operations.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/operations.html)

LDAP 定义了一组操作或请求（参见 [RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)）。在 JNDI 中，这些映射到 [`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) 和 [`LdapContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapContext.html) 接口上的操作（它们是 [`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html) 的子接口）。例如，当调用者调用 `DirContext` 方法时，LDAP 服务提供程序通过向 LDAP 服务器发送 LDAP 请求来实现该方法。

下表显示了 LDAP 中的操作如何对应到 JNDI 方法。

| LDAP 操作 | 对应的 JNDI 方法 |
| --- | --- |
| 绑定 | JNDI 中创建与 LDAP 服务器的初始连接的对应方式是创建一个 [`InitialDirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html)。当应用程序创建初始上下文时，通过环境属性提供客户端身份验证信息。要更改现有上下文的身份验证信息，请使用 [`Context.addToEnvironment()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#addToEnvironment-java.lang.String-java.lang.Object-) 和 [`Context.removeFromEnvironment()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#removeFromEnvironment-java.lang.String-)。 |
| 解绑 | [`Context.close()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#close) 用于释放上下文使用的资源。它与 LDAP 的 "unbind" 操作不同之处在于，在给定的服务提供程序实现中，资源可以在上下文之间共享，因此关闭一个上下文不会释放所有资源，如果这些资源正在与另一个上下文共享。如果您的意图是释放所有资源，请确保关闭所有上下文。 |
| 搜索 | JNDI 中对应的方法是重载 [`DirContext.search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-java.lang.String-java.lang.String-javax.naming.directory.SearchControls-)，接受一个搜索过滤器（[RFC 2254](http://www.ietf.org/rfc/rfc2254.txt)）。查看 过滤器 示例。 |
| 修改 | JNDI 中对应的方法是重载 [`DirContext.modifyAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#modifyAttributes-java.lang.String-javax.naming.directory.ModificationItem:A-)，接受一个 [`DirContext.ModificationItem` 数组](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/ModificationItem.html)。查看 修改属性 部分的示例。 |
| add | JNDI 中对应的方法是[`DirContext.bind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-java.lang.String-java.lang.Object-javax.naming.directory.Attributes-)和[`DirContext.createSubcontext()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#createSubcontext-java.lang.String-javax.naming.directory.Attributes-)。您可以使用其中任一方法来添加新的 LDAP 条目。使用`bind()`，您不仅可以为新条目指定一组属性，还可以指定要与属性一起添加的 Java 对象。请参阅使用 Attributes 添加、替换绑定部分以获取示例。 |
| delete | JNDI 中对应的方法是[`Context.unbind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#unbind-java.lang.String-)和[`Context.destroySubcontext()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#destroySubcontext-java.lang.String-)。您可以使用其中任一方法来删除 LDAP 条目。 |
| modify DN/RDN | JNDI 中对应的方法是[`Context.rename()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rename-java.lang.String-java.lang.String-)。请参阅重命名对象部分以获取更多详细信息。 |
| compare | JNDI 中对应的操作是一个适当受限的[`DirContext.search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-java.lang.String-javax.naming.directory.Attributes-java.lang.String:A-)。请参阅 LDAP 比较部分以获取示例。 |
| abandon | 当您关闭一个上下文时，所有未完成的请求都会被放弃。同样，当您关闭一个[`NamingEnumeration`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html)时，相应的 LDAP“搜索”请求也会被放弃。 |
| extended operation | JNDI 中对应的方法是[`LdapContext.extendedOperation()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapContext.html#extendedOperation-javax.naming.ldap.ExtendedRequest-)。请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/index.html)以获取更多详细信息。 |
