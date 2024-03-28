# 默认 CookieManager

> 原文：[`docs.oracle.com/javase/tutorial/networking/cookies/cookiemanager.html`](https://docs.oracle.com/javase/tutorial/networking/cookies/cookiemanager.html)

[`java.net.CookieManager`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html)提供了`CookieHandler`的具体实现，对于大多数用户来说，足以处理 HTTP 状态管理。`CookieManager`将 cookie 的存储与接受、拒绝的策略分开。`CookieManager`通过[`java.net.CookieStore`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieStore.html)和[`java.net.CookiePolicy`](https://docs.oracle.com/javase/8/docs/api/java/net/CookiePolicy.html)进行初始化。`CookieStore`管理 cookie 的存储。`CookiePolicy`根据策略决定是否接受或拒绝 cookie。

以下代码显示了如何创建并设置系统范围的`CookieManager`：

```java
java.net.CookieManager cm = new java.net.CookieManager();
java.net.CookieHandler.setDefault(cm);

```

第一行调用默认的`CookieManager`构造函数创建实例。第二行调用`CookieHandler`的静态`setDefault`方法来设置系统范围的处理程序。

默认的`CookieManager`构造函数创建一个具有默认 cookie 存储和接受策略的新`CookieManager`实例。`CookieStore`是存储任何接受的 HTTP cookie 的地方。如果在创建时未指定，`CookieManager`实例将使用内部的内存实现。这种实现不是持久的，只存在于 Java 虚拟机的生命周期内。需要持久存储的用户必须实现自己的存储。

`CookieManager`使用的默认 cookie 策略是`CookiePolicy.ACCEPT_ORIGINAL_SERVER`，只接受来自原始服务器的 cookie。因此，服务器的`Set-Cookie`响应必须设置“domain”属性，并且必须与 URL 中的主机域匹配。有关更多信息，请参阅[`java.net.HttpCookie.domainMatches`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpCookie.html#domainMatches-java.lang.String-java.lang.String-)。需要不同策略的用户必须实现`CookiePolicy`接口，并将其传递给`CookieManager`构造函数，或者通过使用`setCookiePolicy(cookiePolicy)`方法将其设置为已构造的`CookieManager`实例。

在从 cookie 存储中检索 cookie 时，`CookieManager`还执行来自[RFC 2965](http://www.ietf.org/rfc/rfc2965.txt)第 3.3.4 节的路径匹配规则。因此，cookie 还必须设置其“path”属性，以便在从 cookie 存储中检索 cookie 之前应用路径匹配规则。

总之，`CookieManager`提供了处理 cookie 的框架，并为`CookieStore`提供了良好的默认实现。通过允许您设置自己的`CookieStore`、`CookiePolicy`或两者，`CookieManager`具有高度的可定制性。
