# 创建

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/create.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/create.html)

创建连接的几种方式。最常见的方式是从创建初始上下文开始。当您使用 LDAP 服务提供程序创建[`InitialContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html)、[`InitialDirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html)或[`InitialLdapContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/InitialLdapContext.html)时，会立即与[`Context.PROVIDER_URL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#PROVIDER_URL)属性中命名的目标 LDAP 服务器建立连接。每次创建初始上下文时，都会创建一个新的 LDAP 连接。有关如何更改此行为的信息，请参见 Pooling 部分。

如果属性值包含多个 URL，则依次尝试每个 URL，直到成功创建连接为止。然后将属性值更新为成功的 URL。有关如何使用 URL 列表创建初始上下文的示例，请参见[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/url.html#MULTI)。

还有三种直接创建连接的方式。

1.  通过将 URL 作为初始上下文的名称参数传递。当将 LDAP 或 LDAPS URL 作为名称参数传递给初始上下文时，URL 中的信息将用于创建到 LDAP 服务器的新连接，而不管初始上下文实例本身是否连接到 LDAP 服务器。实际上，初始上下文可能未连接到任何服务器。有关如何将 URL 用作名称的更多信息，请参见[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/beyond/url/initctx.html)。

1.  另一种创建连接的方式是使用`Reference`。当传递包含 LDAP 或 LDAPS URL 的[`Reference`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Reference.html)给[`NamingManager.getObjectInstance()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/NamingManager.html#getObjectInstance-java.lang.Object-javax.naming.Name-javax.naming.Context-java.util.Hashtable-)或[`DirectoryManager.getObjectInstance()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/DirectoryManager.html#getObjectInstance-java.lang.Object-javax.naming.Name-javax.naming.Context-java.util.Hashtable-javax.naming.directory.Attributes-)时，将使用 URL 中指定的信息创建一个新连接。

1.  最后，当手动或自动跟随引荐时，引荐中的信息将用于创建新连接。有关引荐的信息，请参见[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/index.html)。

## 共享连接

从一个`Context`实例派生的`Context`实例和[`NamingEnumeration`s](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html)将共享相同的连接，直到对其中一个`Context`实例进行更改使共享不再可能。 例如，如果您从初始上下文调用[`Context.lookup()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-)，[`Context.listBindings()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#listBindings-javax.naming.Name-)或[`DirContext.search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-)并获得其他`Context`实例，则所有这些`Context`实例将共享相同的连接。

这里是一个`示例`。

```java
// Create initial context
DirContext ctx = new InitialDirContext(env);

// Get a copy of the same context
Context ctx2 = (Context)ctx.lookup("");

// Get a child context
Context ctx3 = (Context) ctx.lookup("ou=NewHires");

```

在这个示例中，`ctx`，`ctx2`和`ctx3`将共享相同的连接。

共享是无论`Context`实例如何产生都会进行的。 例如，通过遵循引荐获得的`Context`实例将与引荐共享相同的连接。

当您更改与连接相关的`Context`实例的环境属性，例如用户的主体名称或凭据时，您进行这些更改的`Context`实例将获得自己的连接（如果连接是共享的）。 未来从此`Context`实例派生的`Context`实例将共享这个新连接。 先前共享旧连接的`Context`实例不受影响（即它们继续使用旧连接）。

这里是一个使用两个连接的`示例`。

```java
// Create initial context (first connection)
DirContext ctx = new InitialDirContext(env);

// Get a copy of the same context
DirContext ctx2 = (DirContext)ctx.lookup("");

// Change authentication properties in ctx2
ctx2.addToEnvironment(Context.SECURITY_PRINCIPAL, 
    "cn=C. User, ou=NewHires, o=JNDITutorial");
ctx2.addToEnvironment(Context.SECURITY_CREDENTIALS, "mysecret");

// Method on ctx2 will use new connection
System.out.println(ctx2.getAttributes("ou=NewHires"));

```

`ctx2`最初与`ctx`共享相同的连接。 但是当其主体和密码属性更改时，它将无法再使用`ctx`的连接。 LDAP 提供程序将自动为`ctx2`创建一个新连接。

同样，如果您使用[`LdapContext.reconnect()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapContext.html#reconnect-javax.naming.ldap.Control:A-)来更改`Context`实例的连接控制，如果连接正在共享，则`Context`实例将获得自己的连接。

如果`Context`实例的连接未被共享（即没有`Context`派生自它），则对其环境或连接控制的更改不会导致创建新连接。 相反，与连接相关的任何更改将应用于现有连接。

## 创建超时

并非所有连接创建都成功。如果 LDAP 提供程序在一定的超时期限内无法建立连接，则会中止连接尝试。默认情况下，此超时期限是网络（TCP）超时值，大约几分钟。要更改超时期限，您可以使用`"com.sun.jndi.ldap.connect.timeout"`环境属性。此属性的值是表示连接超时的整数的字符串表示。

这里是`一个示例`。

```java
// Set up environment for creating initial context
Hashtable env = new Hashtable(11);
env.put(Context.INITIAL_CONTEXT_FACTORY, 
    "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Specify timeout to be 5 seconds
env.put("com.sun.jndi.ldap.connect.timeout", "5000");

// Create initial context
DirContext ctx = new InitialDirContext(env);

// do something useful with ctx

```

在这个示例中，如果在 5 秒内无法创建连接，将抛出异常。

如果`Context.PROVIDER_URL`属性包含多个 URL，则提供程序将对每个 URL 使用超时。例如，如果有 3 个 URL 并且超时已指定为 5 秒，则提供程序将总共等待最多 15 秒。

请查看连接池部分，了解此属性如何影响连接池。
