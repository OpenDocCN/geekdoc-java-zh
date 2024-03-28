# 标准 LDAP 控件

> 原文：[`docs.oracle.com/javase/tutorial/jndi/newstuff/controls-std.html`](https://docs.oracle.com/javase/tutorial/jndi/newstuff/controls-std.html)

在 LDAP v3 中，控件是通过将其与对服务器或客户端有用的更多信息相关联来增强现有 LDAP 操作的消息。控件可以是请求控件或响应控件。请求控件与 LDAP 请求一起从客户端发送到服务器。响应控件与 LDAP 响应一起从服务器发送到客户端。任何一种都由接口[`javax.naming.ldap.Control`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Control.html)表示。

如果您以前没有使用控件进行编程，请查看[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/)中的控件课程。

在本课程中，我们将讨论添加到 JDK 5.0 的标准 LDAP 控件。必要的 LDAP 控件已经在 JNDI/LDAP 服务提供程序的`com.sun.jndi.ldap.ctl`包下支持的 LDAP Booster Pack 扩展包中支持。由 IETF 标准化的 LDAP 控件现在通过以下类在 JDK 的`javax.naming.ldap`包中提供。

+   [`ManageReferralControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/ManageReferralControl.html) ( [RFC 3296](http://www.ietf.org/rfc/rfc3296.txt) )

+   [`PagedResultsControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/PagedResultsControl.html) ( [RFC 2696](http://www.ietf.org/rfc/rfc2696.txt) )

+   [`PagedResultsResponseControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/PagedResultsResponseControl.html)

+   [`SortControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortControl.html) ( [RFC 2891](http://www.ietf.org/rfc/rfc2891.txt) )

+   [`SortKey`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortKey.html)

+   [`SortResponseControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortResponseControl.html)

+   [`BasicControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/BasicControl.html)
