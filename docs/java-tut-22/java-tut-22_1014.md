# LDAP 非请求通知

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/unsol.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/unsol.html)

LDAP v3（[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)）定义了*非请求通知*，即 LDAP 服务器向客户端发送的无需客户端引发的消息。在 JNDI 中，非请求通知由[`UnsolicitedNotification`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/UnsolicitedNotification.html)接口表示。

由于服务器异步发送非请求通知，您可以使用与接收有关命名空间更改和对象内容更改通知相同的[事件模型](https://docs.oracle.com/javase/jndi/tutorial/beyond/event/index.html)。通过在[`EventContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventContext.html)或[`EventDirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventDirContext.html)上注册[`UnsolicitedNotificationListener`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/UnsolicitedNotificationListener.html)来注册接收非请求通知的兴趣。

这里是`一个示例`展示了`UnsolicitedNotificationListener`的实现。

```java
public class UnsolListener implements UnsolicitedNotificationListener {
    public void notificationReceived(UnsolicitedNotificationEvent evt) {
        System.out.println("received: " + evt);
    }

    public void namingExceptionThrown(NamingExceptionEvent evt) {
        System.out.println(">>> UnsolListener got an exception");
            evt.getException().printStackTrace();
    }
}

```

以下是`一个示例`，展示了如何向事件源注册`UnsolicitedNotificationListener`的实现。请注意，只有[`EventContext.addNamingListener()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventContext.html#addNamingListener-javax.naming.Name-int-javax.naming.event.NamingListener-)中的监听器参数与非请求通知相关。名称和范围参数与非请求通知无关。

```java
// Get the event context for registering the listener
EventContext ctx = (EventContext)
    (new InitialContext(env).lookup("ou=People"));

// Create the listener
NamingListener listener = new UnsolListener();

// Register the listener with the context (all targets equivalent)
ctx.addNamingListener("", EventContext.ONELEVEL_SCOPE, listener);

```

运行此程序时，您需要将其指向一个可以生成非请求通知并促使服务器发出通知的 LDAP 服务器。否则，程序将在一分钟后悄无声息地退出。

实现`UnsolicitedNotificationListener`的监听器也可以实现其他[`NamingListener`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamingListener.html)接口，比如[`NamespaceChangeListener`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamespaceChangeListener.html)和[`ObjectChangeListener`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/ObjectChangeListener.html)。
