# 常见问题

> 译文：[`docs.oracle.com/javase/tutorial/jndi/ldap/faq.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/faq.html)

这节课回答了用户在使用 JNDI 访问 LDAP 服务时经常遇到的常见问题。一些常见问题在命名和目录操作课程的 Trouble Shooting Tips 中得到解答。

* * *

1.  ## 上下文：

1.  上下文是否安全用于多线程访问？

1.  为什么 LDAP 提供程序忽略我的安全环境属性？

1.  为什么我一直收到 CommunicationException？

1.  如何获取 LDAP 消息的跟踪？

1.  如何使用不同的身份验证机制，如 Kerberos？

1.  更改密码时是否应启用 SSL？

    ## 属性：

1.  当我请求一个属性时，为什么返回另一个属性？

1.  如何知道属性值的类型？

1.  如何以除了字符串或字节数组之外的形式获取属性的值？

1.  为什么在我的搜索中将"*"作为属性值不按预期工作？

    ## 搜索：

1.  为什么搜索过滤器中的通配符不总是起作用？

1.  当我知道目录中还有更多条目时，为什么我只返回*n*个条目？

1.  如何在我的搜索中传递控件？

1.  我如何查看返回的搜索结果数量？

    ## 名称：

1.  为什么我的搜索结果中的名称是空字符串？

1.  为什么我的搜索结果中的名称是一个 URL 字符串？

1.  传递给上下文方法的 Name 参数是什么类型？

1.  我可以将从 NameParser 获取的名称传递给 Context 方法吗？

1.  我用于`Context.SECURITY_PRINCIPAL`属性的名称与目录之间有什么关系？

1.  为什么我从目录中读取的名称中有奇怪的引号和转义字符？

1.  如何获取 LDAP 条目的完整 DN？

* * *

1\. 上下文是否安全用于多线程访问，还是需要对上下文进行锁定/同步访问？

答案取决于实现。这是因为[`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html)和[`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html)接口没有指定同步要求。JDK 中的 LDAP 实现针对单线程访问进行了优化。如果有多个线程访问相同的`Context`实例，则每个线程在使用时需要锁定`Context`实例。这也适用于从相同`Context`实例派生的任何`NamingEnumeration`。然而，多个线程可以同时访问*不同*的`Context`实例（甚至是从同一个初始上下文派生的实例）而无需锁定。

2\. 如果我不设置[`Context.SECURITY_CREDENTIALS`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_CREDENTIALS) (`"java.naming.security.credentials"`)属性或将其设置为空字符串，为什么 LDAP 提供程序会忽略我的安全环境属性？

如果您向`Context.SECURITY_CREDENTIALS`环境属性提供空字符串、空的`byte`/`char`数组或`null`，即使`Context.SECURITY_AUTHENTICATION`属性设置为`"simple"`，也会发生匿名绑定。这是因为对于简单认证，LDAP 要求密码不能为空。如果未提供密码，则协议会自动将认证转换为`"none"`。

3\. 当我尝试创建初始上下文时为什么会一直收到[`CommunicationException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/CommunicationException.html)？

您可能正在与仅支持 LDAP v2 的服务器通信。请参考[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/version.html)中的杂项课程，了解如何设置版本号的示例。

4\. 我如何追踪 LDAP 消息？

尝试使用`"com.sun.jndi.ldap.trace.ber"`环境属性。如果此属性的值是`java.io.OutputStream`的实例，则 LDAP 提供程序发送和接收的 BER 缓冲区的跟踪信息将写入该流。如果属性的值为`null`，则不会写入跟踪输出。

例如，以下代码将把跟踪输出发送到`System.err`。

```java
env.put("com.sun.jndi.ldap.trace.ber", System.err);

```

5\. 我如何使用不同的认证机制，比如 Kerberos？

参考[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/gssapi.html)中的 GSS-API/Kerberos v5 认证部分，了解如何使用 Kerberos 认证。要使用其他认证机制，请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/mechanism.html)中的使用任意 SASL 机制部分。

6\. 在更改密码时应该启用 SSL 吗？

这实际取决于您正在使用的目录服务器。一些目录服务器如果未启用 SSL，则不允许您更改密码，但有些允许。启用 SSL 可以确保密码在通信通道中得到保护。

7\. 当我请求一个属性时，为什么会返回另一个属性？

您正在使用的属性名称可能是另一个属性的同义词。在这种情况下，LDAP 服务器可能返回规范属性名称而不是您提供的属性名称。当您查看服务器返回的`Attributes`时，需要使用规范名称而不是同义词。

例如，`"fax"`可能是规范属性名称`"facsimiletelephonenumber"`的同义词。如果您请求`"fax"`，服务器将返回名为`"facsimiletelephonenumber"`的属性。有关同义词和属性名称的其他问题，请参阅 Naming and Directory Operations 课程。

8\. 我如何知道属性值的类型？

属性值可以是`java.lang.String`或`byte[]`。请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/attrs.html)的杂项部分，了解哪些属性值以`byte[]`形式返回。要以编程方式执行此操作，您可以使用`instanceof`运算符检查从 LDAP 提供程序返回的属性值。

9\. 如何以字符串或字节数组以外的形式获取属性值？

目前你不能。LDAP 提供程序仅返回`java.lang.String`或`byte[]`类型的属性值。请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/attrs.html)的杂项部分。

10\. 为什么在我的搜索中将"*"作为属性值不起作用？

当您使用以下形式的`search()`时，属性值被视为文字；也就是说，目录条目中的属性应该正好包含该值：[`search(Name name, Attributes matchingAttrs)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-) 要使用通配符，您应该使用`search()`的字符串过滤形式，如下所示。[`search(Name name, String filter, SearchControls ctls)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-)

[`search(Name name, String filterExpr, Object[]filterArgs, SearchControls ctls)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-java.lang.Object:A-javax.naming.directory.SearchControls-) 

对于最后一种形式，通配符字符必须出现在`filterExpr`参数中，而不是在`filterArgs`中。`filterArgs`中的值也被视为文字。

11\. 为什么搜索过滤器中的通配符不总是起作用？

通配符出现在属性值之前或之后（例如在`"attr=*I*"`中）表示服务器将使用属性的子字符串匹配规则搜索匹配的属性值。如果属性的定义没有子字符串匹配规则，则服务器无法找到属性。您必须使用相等或“存在”过滤器进行搜索。

12\. 当我知道目录中还有更多条目时，为什么我只返回了*n*个条目？一些服务器配置为限制可以返回的条目数。其他服务器在搜索期间也限制可以检查的条目数。请检查您的服务器配置。

13\. 我如何在我的搜索中传递控件？

本教程中未解释控件。查看[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/context.html)。

14\. 我如何知道我得到了多少搜索结果？

在枚举结果时必须进行计数。LDAP 不提供这些信息。

15\. 为什么我在我的[`SearchResult`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchResult.html)中得到一个空字符串作为名称？

[`getName()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getName--) 总是返回一个相对于搜索的*目标上下文*的名称。因此，如果目标上下文满足搜索过滤器，则返回的名称将是""（空名称），因为这是相对于目标上下文的名称。有关详细信息，请参阅搜索结果部分。

16\. 为什么我在我的[`SearchResult`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchResult.html)中得到一个 URL 字符串作为名称？

LDAP 条目是通过跟随别名或引荐而检索的，因此它的名称是一个 URL。有关详细信息，请参阅搜索结果课程。

17\. 传递给[`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html)和[`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html)方法的[`Name`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Name.html)参数是什么类型？- 一个[`CompoundName`](https://docs.oracle.com/javase/8/docs/api/javax/naming/CompoundName.html)还是一个[`CompositeName`](https://docs.oracle.com/javase/8/docs/api/javax/naming/CompositeName.html)？

字符串形式接受复合名称的字符串表示。也就是说，使用字符串名称等同于调用`new CompositeName(stringName)`并将结果传递给`Context`/`DirContext`方法。`Name`参数可以是实现`Name`接口的任何对象。如果它是`CompositeName`的一个实例，则该名称被视为复合名称；否则，它被视为复合名称。

18\. 我可以将从[`NameParser`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameParser.html)得到的名称传递给[`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html)方法吗？

这与前一个问题有关。是的，你可以。[`NameParser.parse()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameParser.html#parse-java.lang.String-) 返回一个实现了`Name`接口的复合名称。这个名称可以传递给`Context`方法，这些方法将把它解释为一个复合名称。

19\. 我在[`Context.SECURITY_PRINCIPAL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL)属性中使用的名称与目录之间有什么关系？

你可以将主要名称视为来自不同命名空间的名称。参见[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)和安全部分，了解 LDAP 认证机制的详细信息。JDK 中的 LDAP 服务提供程序接受一个字符串主要名称，直接传递给 LDAP 服务器。一些 LDAP 服务器接受 DN，而其他一些支持[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)提出的方案。

20\. 为什么我从目录中读取的名称中会有奇怪的引号和转义字符？

JDK 中的 LDAP 名称解析器在引号规则方面比较保守，但仍然生成“正确”的名称。另外，请记住，[`NamingEnumeration`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html)返回的条目名称是可以传递回`Context`和`DirContext`方法的复合名称。因此，如果名称中包含与复合名称语法冲突的字符（例如斜杠字符“/”），那么 LDAP 提供程序将提供一种编码，以确保斜杠字符将被视为 LDAP 名称的一部分，而不是复合名称分隔符。

开始使用[`LdapName`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html)和[`Rdn`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html)类，实现简单的名称操作。

21\. 如何获取 LDAP 条目的完整 DN？

您可以使用[`NameClassPair.getNameInNamespace()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getNameInNamespace--)。

* * *
