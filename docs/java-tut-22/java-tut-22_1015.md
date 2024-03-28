# 连接管理

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/connect.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/connect.html)

JNDI 为访问命名和目录服务提供了高级接口。JNDI [`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html)实例与底层网络连接之间的映射可能不是一对一的。只要保留接口语义，服务提供者就可以自由共享和重用连接。应用程序开发人员通常不需要了解`Context`实例如何创建和使用连接的细节。当开发人员需要调整程序时，这些细节是有用的。

本课程描述了 LDAP 服务提供者如何使用连接。它描述了何时创建连接以及如何指定特殊的连接参数，例如多个服务器和连接超时。本课程还展示了如何在支持的网络环境中动态发现和使用 LDAP 服务器。

创建的连接最终必须关闭。本课程包含一个描述客户端和服务器如何进行连接关闭的部分。

最后，本课程向您展示如何使用连接池使使用许多短暂连接的应用程序更有效率。

* * *

**注意：** 本课程中提供的信息仅适用于 JDK 中的 LDAP 服务提供者。其他供应商的 LDAP 服务提供者可能不使用相同的连接管理策略。

* * *
