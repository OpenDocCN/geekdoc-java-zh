# 关闭

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/close.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/close.html)

正常的垃圾回收会在不再使用时删除`Context`实例。被垃圾回收的`Context`实例使用的连接将自动关闭。因此，您无需显式关闭连接。然而，网络连接是有限资源，对于某些程序，您可能希望控制它们的增殖和使用。本节包含如何关闭连接以及如何在服务器关闭连接时收到通知的信息。

## 显式关闭

您可以调用[`Context.close()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#close--)来指示您不再需要使用`Context`实例。如果要关闭的`Context`实例正在使用专用连接，则该连接也将关闭。如果`Context`实例正在与其他`Context`和未终止的`NamingEnumeration`实例共享连接，则在所有这些`Context`和`NamingEnumeration`实例上调用`close()`之前，连接将不会关闭。

在连接创建示例部分的`example`中，必须在关闭底层连接之前关闭所有三个`Context`实例。

```java
// Create initial context
DirContext ctx = new InitialDirContext(env);

// Get a copy of the same context
Context ctx2 = (Context)ctx.lookup("");

// Get a child context
Context ctx3 = (Context) ctx.lookup("ou=NewHires");

// do something useful with ctx, ctx2, ctx3

// Close the contexts when we're done
ctx.close();
ctx2.close();
ctx3.close();

```

## 强制隐式闭包

如前所述，对于不再在作用域内的`Context`和`NamingEnumeration`实例，Java 运行时系统最终会对它们进行垃圾回收，从而清理`close()`将要执行的状态。要强制进行垃圾回收，您可以使用以下代码。

```java
Runtime.getRuntime().gc();
Runtime.getRuntime().runFinalization();

```

根据程序的状态，执行此过程可能会导致严重（临时）性能下降。如果需要确保连接已关闭，请跟踪`Context`实例并显式关闭它们。

## 检测连接关闭

LDAP 服务器通常在空闲超时后关闭不再使用的连接。当您随后在使用此连接的`Context`实例上调用方法时，该方法将抛出[`CommunicationException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/CommunicationException.html)。要检测服务器关闭`Context`实例正在使用的连接时，您需要在`Context`实例上注册一个[`UnsolicitedNotificationListener`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/UnsolicitedNotificationListener.html)。在 LDAP 未经请求通知部分显示了一个`示例`。尽管该示例设计用于从服务器接收未经请求的通知，但也可用于检测服务器关闭连接。启动程序后，停止 LDAP 服务器并观察监听器的[`namingExceptionThrown()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamingListener.html#namingExceptionThrown-javax.naming.event.NamingExceptionEvent-)方法被调用。
