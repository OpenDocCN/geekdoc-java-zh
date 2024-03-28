# 与安全管理器的关系

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/properties/security.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/security.html)

在尝试连接之前，将首先检查 JAXP 属性，无论是否存在`SecurityManager`。这意味着即使`SecurityManager`授予权限，连接也可能被阻止。例如，如果 JAXP 属性被设置为禁止 http 协议，它们将有效地阻止任何连接尝试，即使应用程序具有`SocketPermission`。

为了限制连接，`SecurityManager`可以被视为处于较低级别。在评估 JAXP 属性之后，权限将被检查。例如，如果一个应用程序没有`SocketPermission`，即使 JAXP 属性被设置为允许 http 连接，也会抛出`SecurityException`。

当存在`SecurityManager`时，`JAXP FEATURE_SECURE_PROCESSING`被设置为 true。这种行为不会启用新的限制。
