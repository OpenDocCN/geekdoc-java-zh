# 配置

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/config.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/config.html)

连接池是针对每个 Java 运行时配置和维护的。连接不会跨不同的运行时共享。要使用连接池，不需要任何配置。只有在想要自定义池化方式时才需要配置，比如控制池的大小和哪些类型的连接被池化。

你可以在程序启动时通过一些系统属性来配置连接池。请注意这些是系统属性，*不是*环境属性，并且它们影响所有连接池请求。

这是一个设置最大池大小为 20，首选池大小为 10，并且空闲超时为一分钟的池化连接的命令行示例。

```java
# java -Dcom.sun.jndi.ldap.connect.pool.maxsize=20 \
       -Dcom.sun.jndi.ldap.connect.pool.prefsize=10 \
       -Dcom.sun.jndi.ldap.connect.pool.timeout=60000 \
    UsePool

```

下表列出了用于配置连接池的系统属性。它们在本节的其余部分中有更详细的描述。

| 系统属性名称 | 描述 | 默认值 |
| --- | --- | --- |
| `com.sun.jndi.ldap.connect.pool.authentication` | 一个以空格分隔的连接认证类型列表，可以被池化。有效类型为"none"、"simple"和"DIGEST-MD5"。 | `"none simple"` |
| `com.sun.jndi.ldap.connect.pool.debug` | 一个指示要生成的调试输出级别的字符串。有效值为`"fine"`（跟踪连接的创建和移除）和"all"（所有调试信息）。 |   |
| `com.sun.jndi.ldap.connect.pool.initsize` | 一个整数的字符串表示，表示在为标识创建连接时初始创建的每个连接标识的连接数。 | 1 |
| `com.sun.jndi.ldap.connect.pool.maxsize` | 一个整数的字符串表示，表示每个连接标识可以同时维护的最大连接数。 | 无最大大小 |
| `com.sun.jndi.ldap.connect.pool.prefsize` | 一个整数的字符串表示，表示应同时维护的每个连接标识的首选连接数。 | 无首选大小 |
| `com.sun.jndi.ldap.connect.pool.protocol` | 一个以空格分隔的连接协议类型列表，可以被池化。有效类型为"plain"和"ssl"。 | `"plain"` |
| `com.sun.jndi.ldap.connect.pool.timeout` | 一个整数的字符串表示，表示空闲连接在池中可以保持的毫秒数，超过这个时间将被关闭并从池中移除。 | 无超时 |

## 被池化的内容

当您请求`Context`实例使用连接池时，可以通过使用`"com.sun.jndi.ldap.connect.pool"`环境属性，所使用的连接可能会被池化，也可能不会。默认规则是允许使用简单或无身份验证的普通（非 SSL）连接进行池化。您可以通过使用系统属性来更改此默认设置，以包括 SSL 连接和 DIGEST-MD5 身份验证类型。要允许普通和 SSL 连接都被池化，请将`"com.sun.jndi.ldap.connect.pool.protocol"`系统属性设置为字符串`"plain ssl"`。要允许匿名（无）、简单和 DIGEST-MD5 身份验证类型的连接被池化，请将`com.sun.jndi.ldap.connect.pool.authentication`系统属性设置为字符串`"none simple DIGEST-MD5"`。

有几个环境属性会自动使`Context`实例无法使用池化连接。如果`Context`实例将其`"java.naming.ldap.factory.socket"`属性设置为自定义套接字工厂类，或将其`"java.naming.security.sasl.callback"`属性设置为自定义回调处理程序类，或将其`"com.sun.jndi.ldap.trace.ber"`属性设置为启用协议跟踪，则`Context`实例无法使用池化连接。

## 连接是如何被池化的

当`Context`实例请求使用池化连接时，LDAP 提供程序需要确定是否可以通过现有的池化连接满足请求。它通过为每个池化连接分配*连接标识*并检查传入请求是否具有与其池化连接之一的连接标识相同来实现这一点。

连接标识是创建可能经过身份验证的 LDAP 连接所需的参数集。其组成取决于请求的身份验证类型，如下表所示。

| 身份验证类型 | 连接标识内容 |
| --- | --- |
| none |

+   连接控制

+   主机名、端口号如在`"java.naming.provider.url"`属性中指定的，引荐或提供给初始上下文的 URL。

+   以下属性的内容：

    ```java
    java.naming.security.protocol
    java.naming.ldap.version

    ```

|

| simple |
| --- |

+   所有列出的关于无身份验证的信息

+   以下属性的内容：

    ```java
    java.naming.security.principal
    java.naming.security.credentials

    ```

|

| DIGEST-MD5 |
| --- |

+   所有列出的关于简单身份验证的信息

+   以下属性的内容：

    ```java
    java.naming.security.sasl.authorizationId
    java.naming.security.sasl.realm
    javax.security.sasl.qop
    javax.security.sasl.strength
    javax.security.sasl.server.authentication
    javax.security.sasl.maxbuffer
    javax.security.sasl.policy.noplaintext
    javax.security.sasl.policy.noactive
    javax.security.sasl.policy.nodictionary
    javax.security.sasl.policy.noanonymous
    javax.security.sasl.policy.forward
    javax.security.sasl.policy.credentials

    ```

|

## 池大小

LDAP 提供程序维护连接池；每个池保存具有相同连接标识的连接（正在使用或空闲）。有三个大小影响每个池的管理。这些大小是全局的，影响所有池。

*初始池大小*是 LDAP 服务提供程序在首次创建池时（即应用程序首次为该连接标识请求池化连接时）创建的每个连接标识的连接数。池中每个连接的身份验证是按需执行的，随着连接的使用而进行。默认情况下，初始池大小为 1，并且可以通过使用系统属性`"com.sun.jndi.ldap.connect.pool.initsize"`进行更改。通常在应用程序启动时使用，以向服务器预先提供一定数量的连接。

*最大池大小*是 LDAP 服务提供程序可以同时维护的每个连接标识的最大连接数。使用中和空闲连接都计入此数字。当池大小达到此数字时，对应连接标识的新连接将无法创建，直到池中的连接被移除（即物理连接被关闭）。当池大小达到最大值且池中的所有连接都在使用中时，应用程序从该池请求连接时将被阻塞，直到池中的连接变为空闲或被移除。最大池大小为 0 意味着没有最大大小：对池化连接的请求将使用现有的空闲池化连接或新创建的池化连接。

*首选池大小*是 LDAP 服务提供程序应该维护的每个连接标识的首选连接数。使用中和空闲连接都计入此数字。当应用程序请求使用池化连接且池大小小于首选大小时，LDAP 提供程序将创建并使用新的池化连接，而不管是否有空闲连接可用。当应用程序完成对池化连接的使用（通过在共享连接上调用`Context.close()`）且池大小大于首选大小时，LDAP 提供程序将关闭并从池中移除池化连接。首选池大小为 0 意味着没有首选大小：对池化连接的请求只会在没有空闲连接可用时才会创建新连接。

请注意，最大池大小会覆盖初始和首选池大小。例如，将首选池大小设置为大于最大池大小实际上是将其设置为最大池大小。

## 空闲连接

当应用程序完成对池化连接的使用（通过在共享连接的所有上下文上调用`Context.close()`），底层的池化连接被标记为空闲，等待重新使用。默认情况下，空闲连接会一直保留在池中，直到被垃圾回收。如果设置了`"com.sun.jndi.ldap.connect.pool.timeout"`系统属性，LDAP 提供程序将自动关闭并移除空闲时间超过指定时期的池化连接。
