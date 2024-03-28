# LDAP 的认证方式

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/authentication.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/authentication.html)

在 LDAP 中，认证信息是在"bind"操作中提供的。在 LDAP v2 中，客户端通过发送包含认证信息的"bind"操作与 LDAP 服务器建立连接。

在 LDAP v3 中，此操作具有相同的目的，但是是可选的。发送 LDAP 请求而不执行"bind"操作的客户端被视为*匿名*客户端（有关详细信息，请参见匿名部分）。在 LDAP v3 中，"bind"操作可以在连接期间的任何时候发送，可能会多次发送。客户端可以在连接的中间发送"bind"请求以更改其身份。如果请求成功，则所有使用旧身份的连接上的未完成请求都将被丢弃，并且连接将与新身份关联。

在"bind"操作中提供的认证信息取决于客户端选择的*认证机制*。有关认证机制的讨论，请参见认证机制。

## 使用 JNDI 进行 LDAP 认证

在 JNDI 中，认证信息是在环境属性中指定的。当您使用[`InitialDirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html)类（或其超类或子类）创建初始上下文时，您提供一组环境属性，其中一些可能包含认证信息。您可以使用以下环境属性来指定认证信息。

+   [`Context.SECURITY_AUTHENTICATION`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION) (`"java.naming.security.authentication"`).

    指定要使用的认证机制。对于 JDK 中的 LDAP 服务提供程序，这可以是以下字符串之一："none"，"simple"，*sasl_mech*，其中*sasl_mech*是一组以空格分隔的 SASL 机制名称。有关这些字符串的描述，请参见认证机制。

+   [`Context.SECURITY_PRINCIPAL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL) (`"java.naming.security.principal"`).

    指定进行认证的用户/程序的名称，取决于`Context.SECURITY_AUTHENTICATION`属性的值。有关详细信息和示例，请参见本课程的接下来几节。

+   [`Context.SECURITY_CREDENTIALS`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_CREDENTIALS) (`"java.naming.security.credentials"`).

    指定进行认证的用户/程序的凭据，取决于`Context.SECURITY_AUTHENTICATION`属性的值。有关详细信息和示例，请参见本课程的接下来几节。

创建初始上下文时，底层 LDAP 服务提供程序从这些环境属性中提取认证信息，并使用 LDAP 的“绑定”操作将其传递给服务器。

`以下示例`展示了如何通过使用简单的明文密码，客户端向 LDAP 服务器进行身份验证。

```java
// Set up the environment for creating the initial context
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Authenticate as S. User and password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

## 为上下文使用不同的认证信息

如果要为现有上下文使用不同的认证信息，则可以使用[`Context.addToEnvironment()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#addToEnvironment-java.lang.String-java.lang.Object-)和[`Context.removeFromEnvironment()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#removeFromEnvironment-java.lang.String-)来更新包含认证信息的环境属性。随后对上下文的方法调用将使用新的认证信息与服务器通信。

`以下示例`展示了如何在创建上下文后将上下文的认证信息更改为`"none"`。

```java
// Authenticate as S. User and the password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

// Change to using no authentication
ctx.addToEnvironment(Context.SECURITY_AUTHENTICATION, "none");

// ... do something useful with ctx

```

## 认证失败

认证可能因多种原因而失败。例如，如果提供了不正确的认证信息，比如不正确的密码或主体名称，那么会抛出[`AuthenticationException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationException.html)。

这里是一个变体的`示例`。这次，不正确的密码导致认证失败。

```java
// Authenticate as S. User and give an incorrect password
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "notmysecret");

```

这将产生以下输出。

```java
javax.naming.AuthenticationException: [LDAP: error code 49 - Invalid Credentials]
        ...

```

因为不同的服务器支持不同的认证机制，您可能请求服务器不支持的认证机制。在这种情况下，将抛出[`AuthenticationNotSupportedException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationNotSupportedException.html)。

这里是一个变体的`示例`。这次，不支持的认证机制（`"custom"`）导致认证失败。

```java
// Authenticate as S. User and the password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "custom");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

```

这将产生以下输出。

```java
javax.naming.AuthenticationNotSupportedException: custom
        ...

```
