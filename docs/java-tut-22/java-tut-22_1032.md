# 设置 LDAP 操作的超时时间

> 原文：[`docs.oracle.com/javase/tutorial/jndi/newstuff/readtimeout.html`](https://docs.oracle.com/javase/tutorial/jndi/newstuff/readtimeout.html)

当客户端向服务器发出 LDAP 请求并且服务器由于某种原因未响应时，客户端将永远等待服务器响应，直到 TCP 超时。在客户端端，用户体验实际上是一个进程挂起。为了及时控制 LDAP 请求，自 Java SE 6 以来可以为 JNDI/LDAP 服务提供程序配置读取超时。

新的环境属性：

`com.sun.jndi.ldap.read.timeout`

可用于指定 LDAP 操作的读取超时时间。该属性的值是表示 LDAP 操作的读取超时时间（以毫秒为单位）的整数的字符串表示。如果 LDAP 提供程序在指定的时间内未收到 LDAP 响应，则会中止读取尝试。整数应大于零。小于或等于零的整数表示未指定读取超时，相当于无限等待响应直到接收到响应，这是默认行为。

如果未指定此属性，默认情况下将等待响应直到接收到响应。

例如，`env.put("com.sun.jndi.ldap.read.timeout", "5000");` 会导致 LDAP 服务提供程序在服务器在 5 秒内未响应时中止读取尝试。

这里有一个示例，`ReadTimeoutTest`，使用一个不响应 LDAP 请求的虚拟服务器，展示了当将此属性设置为非零值时的行为。

```java
env.put(Context.INITIAL_CONTEXT_FACTORY,
        "com.sun.jndi.ldap.LdapCtxFactory");
env.put("com.sun.jndi.ldap.read.timeout", "1000");
env.put(Context.PROVIDER_URL, "ldap://localhost:2001");

Server s = new Server();

try {

    // start the server
    s.start();

   // Create initial context
   DirContext ctx = new InitialDirContext(env);
   System.out.println("LDAP Client: Connected to the Server");
        :
        :
} catch (NamingException e) {
   e.printStackTrace();
}

```

由于服务器甚至未响应 LDAP 绑定请求，当创建 InitialDirContext 时，上述程序打印下面的堆栈跟踪。客户端在等待服务器响应时超时。

```java
Server: Connection accepted
javax.naming.NamingException: LDAP response read timed out, timeout used:1000ms.
:
:

at javax.naming.directory.InitialDirContext.<init>(InitialDirContext.java:82)
at ReadTimeoutTest.main(ReadTimeoutTest.java:32)

```

请注意，此属性与另一个环境属性 `com.sun.jndi.ldap.connect.timeout` 不同，后者设置连接到服务器的超时时间。读取超时适用于与服务器建立初始连接后从服务器接收的 LDAP 响应。
