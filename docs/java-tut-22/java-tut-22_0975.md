# 目录和 LDAP 包

> 原文：[`docs.oracle.com/javase/tutorial/jndi/overview/dir.html`](https://docs.oracle.com/javase/tutorial/jndi/overview/dir.html)

## 目录包

[`javax.naming.directory`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/package-summary.html)包扩展了[`javax.naming`](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-summary.html)包，提供了访问目录服务以及命名服务的功能。该包允许应用程序检索存储在目录中的对象相关的属性，并使用指定的属性搜索对象。

### 目录上下文

[`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html)接口代表一个*目录上下文*。`DirContext`还通过扩展[`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html)接口来充当命名上下文。这意味着任何目录对象也可以提供命名上下文。它定义了用于检查和更新与目录条目关联的属性的方法。

属性

您可以使用[`getAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax.naming/directory/DirContext.html#getAttributes-javax.naming.Name-)方法检索与目录条目关联的属性（您提供名称）。使用[`modifyAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax.naming/directory/DirContext.html#modifyAttributes-javax.naming.Name-javax.naming.directory.ModificationItem:A-)方法修改属性。您可以使用此操作添加、替换或删除属性和/或属性值。

搜索

`DirContext`包含用于执行基于内容的目录搜索的方法。在最简单和最常见的用法中，应用程序指定一组可能具有特定值的属性，并将此属性集提交给[`search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-)方法。其他重载形式的[`search()`](https://docs.oracle.com/javase/8/docs/api/javax.naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-)支持更复杂的搜索过滤器。

## LDAP 包

[`javax.naming.ldap`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/package-summary.html) 包含了用于使用特定于[LDAP v3](http://www.ietf.org/rfc/rfc2251.txt)的功能的类和接口，这些功能不在更通用的[`javax.naming.directory`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/package-summary.html)包中。事实上，大多数使用 LDAP 的 JNDI 应用程序会发现`javax.naming.directory`包已经足够，并且根本不需要使用`javax.naming.ldap`包。这个包主要是为那些需要使用“扩展”操作、控件或未经请求的通知的应用程序而设计的。

“扩展”操作

除了指定了搜索和修改等明确定义的操作之外，[LDAP v3 (RFC 2251)](http://www.ietf.org/rfc/rfc2251.txt)还指定了在 LDAP 客户端和服务器之间传输尚未定义操作的方法。这些操作被称为*“扩展”操作*。一个“扩展”操作可以由标准组织（如互联网工程任务组）或供应商定义。

控件

[LDAP v3](http://www.ietf.org/rfc/rfc2251.txt)允许任何请求或响应通过尚未定义的修饰符进行增强，称为*控件*。与请求一起发送的控件是*请求控件*，与响应一起发送的控件是*响应控件*。控件可以由标准组织（如 IETF）或供应商定义。请求控件和响应控件不一定是成对的，也就是说，并非每个发送的请求控件都需要有相应的响应控件，反之亦然。

未经请求的通知

除了客户端和服务器之间的正常请求/响应交互方式之外，[LDAP v3](http://www.ietf.org/rfc/rfc2251.txt)还指定了*未经请求的通知*--这些消息是从服务器异步发送给客户端的，而不是响应于任何客户端请求。

### LDAP 上下文

[`LdapContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapContext.html) 接口代表了执行“扩展”操作、发送请求控件和接收响应控件的*上下文*。如何使用这些功能的示例在 JNDI 教程的[控件和扩展](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/index.html)课程中有描述。
