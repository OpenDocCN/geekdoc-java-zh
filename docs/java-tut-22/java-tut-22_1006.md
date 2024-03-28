# 匿名

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/anonymous.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/anonymous.html)

正如刚才所述，如果没有设置任何身份验证环境属性，那么默认的身份验证机制是`"none"`。如果客户端将[`Context.SECURITY_AUTHENTICATION`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION)环境属性设置为`"none"`，那么身份验证机制就是`"none"`，所有其他身份验证环境属性都将被忽略。您只有在明确希望忽略可能已设置的任何其他身份验证属性时才需要这样做。无论哪种情况，客户端都将被视为*匿名*客户端。这意味着服务器不知道也不关心客户端是谁，并且将允许客户端访问（读取和更新）任何已配置为可被任何未经身份验证的客户端访问的数据。

因为 Naming and Directory Operations 课程中的所有目录示例都没有设置任何身份验证环境属性，所以它们都使用匿名身份验证。

这里是一个`示例`，明确将`Context.SECURITY_AUTHENTICATION`属性设置为`"none"`（尽管这样做并不是严格必要的，因为这是默认值）。

```java
// Set up the environment for creating the initial context
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Use anonymous authentication
env.put(Context.SECURITY_AUTHENTICATION, "none");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```
