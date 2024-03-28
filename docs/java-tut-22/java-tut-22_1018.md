# 连接池

> 译文：[`docs.oracle.com/javase/tutorial/jndi/ldap/pool.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/pool.html)

连接创建部分描述了何时创建连接。它描述了多个`Context`实例如何共享相同的连接。

LDAP 服务提供程序支持的另一种连接共享类型称为*连接池*。在这种共享类型中，LDAP 服务提供程序维护一组（可能是）先前使用过的连接，并根据需要将它们分配给`Context`实例。当`Context`实例完成连接（关闭或垃圾回收）时，连接将被返回到池中以供将来使用。请注意，这种共享形式是顺序的：从池中检索连接，使用连接，将连接返回到池中，然后再次从池中检索连接以供另一个`Context`实例使用。

连接池是针对每个 Java 运行时系统进行维护的。在某些情况下，使用连接池可以显著提高性能。例如，如果使用连接池，则处理包含对同一 LDAP 服务器的四个引用引用的搜索响应仅需要一个连接。如果没有使用连接池，这种情况将需要四个单独的连接。

本课程的其余部分将更详细地描述如何使用连接池。

## 如何使用连接池

通过向传递给初始上下文构造函数的环境属性添加属性`"com.sun.jndi.ldap.connect.pool"`来请求连接池。这里是`一个示例`。

```java
// Set up environment for creating initial context
Hashtable env = new Hashtable(11);
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Enable connection pooling
env.put("com.sun.jndi.ldap.connect.pool", "true");

// Create one initial context (Get connection from pool)
DirContext ctx = new InitialDirContext(env);

// do something useful with ctx

// Close the context when we're done
ctx.close();   // Return connection to pool

// Create another initial context (Get connection from pool)
DirContext ctx2 = new InitialDirContext(env);

// do something useful with ctx2

// Close the context when we're done
ctx2.close();   // Return connection to pool

```

此示例连续创建两个初始上下文。第二个初始上下文将重用第一个使用的连接。要运行此程序并观察如何检索连接并将其返回到池中，请使用以下命令行。

```java
#java -Dcom.sun.jndi.ldap.connect.pool.debug=fine UsePool

```

这应该产生以下输出。

```java
Create com.sun.jndi.ldap.LdapClient@5d173[localhost:389]
Use com.sun.jndi.ldap.LdapClient@5d173
{ou=ou: NewHires, objectclass=objectClass: top, organizationalUnit}
Release com.sun.jndi.ldap.LdapClient@5d173
Use com.sun.jndi.ldap.LdapClient@5d173
{ou=ou: People, objectclass=objectClass: top, organizationalunit}
Release com.sun.jndi.ldap.LdapClient@5d173

```

通过包含或省略`"com.sun.jndi.ldap.connect.pool"`属性，您可以决定何时何地使用连接池，从而在*每个上下文*的基础上控制连接池。在前面的示例中，如果在创建第二个初始上下文之前从环境属性中删除此属性，则第二个初始上下文将不使用池化连接。

LDAP 提供程序通过应用程序的指示来跟踪连接是否正在使用。它假定维护打开上下文句柄的应用程序正在使用连接。因此，为了使 LDAP 提供程序正确管理池化连接，您必须勤于在不再需要的上下文上调用`Context.close()`。

坏连接会被 LDAP 提供程序自动检测并从池中移除。无论是否使用连接池，上下文最终使用坏连接的概率是相同的。

## 创建超时

LDAP 服务提供程序维护的连接池可能具有限制的大小；这在连接池配置部分有详细描述。当启用连接池并且没有可用的池化连接时，客户端应用程序将被阻塞，等待可用连接。您可以使用`"com.sun.jndi.ldap.connect.timeout"`环境属性来指定等待池化连接的时间。如果省略此属性，应用程序将无限期等待。

此属性还用于指定建立 LDAP 连接的超时期限，如连接创建部分所述。

## 何时不使用池化！！

池化连接旨在被重复使用。因此，如果您计划对可能改变底层连接状态的`Context`实例执行操作，则不应该对该`Context`实例使用连接池。例如，如果您计划在`Context`实例上调用 Start TLS 扩展操作，或者计划在创建初始上下文后更改安全相关属性（如`"java.naming.security.principal"`或`"java.naming.security.protocol"`），则不应该对该`Context`实例使用连接池，因为 LDAP 提供程序不会跟踪任何此类状态更改。在这种情况下使用连接池，可能会 compromise 您的应用程序的安全性。
