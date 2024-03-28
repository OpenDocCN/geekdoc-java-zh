# 搜索结果

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/result.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/result.html)

当您在[`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html)接口中使用搜索方法时，您将获得一个[`NamingEnumeration`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html)。`NamingEnumeration`中的每个项目都是一个[`SearchResult`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchResult.html)，其中包含以下信息：

+   名称

+   对象

+   类名

+   属性

## 名称

每个`SearchResult`包含满足搜索过滤器的 LDAP 条目的名称。您可以通过使用[`getName()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getName--)来获取条目的名称。该方法返回 LDAP 条目相对于*目标上下文*的[`复合名称`](https://docs.oracle.com/javase/8/docs/api/javax/naming/CompositeName.html)。目标上下文是`name`参数解析到的上下文。在 LDAP 术语中，目标上下文是搜索的*基本对象*。以下是一个示例。

```java
NamingEnumeration answer = ctx.search("ou=NewHires", 
    "(&(mySpecialKey={0}) (cn=*{1}))",  // Filter expression
    new Object[]{key, name},                // Filter arguments
    null);                                  // Default search controls

```

在本示例中，目标上下文由`"ou=NewHires"`命名。`answer`中的`SearchResult`中的名称相对于`"ou=NewHires"`。例如，如果`getName()`返回`"cn=J. Duke"`，那么相对于`ctx`的名称将是`"cn=J. Duke, ou=NewHires"`。

如果您使用[`SearchControls.SUBTREE_SCOPE`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#SUBTREE_SCOPE)或[`SearchControls.OBJECT_SCOPE`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#OBJECT_SCOPE)执行搜索，并且目标上下文本身满足搜索过滤器，则返回的名称将是""（空名称），因为这是相对于目标上下文的名称。

这并不是全部。如果搜索涉及引荐（请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/index.html)）或解引用别名（请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/aliases.html)），那么相应的`SearchResult`将具有不相对于目标上下文的名称。相反，它们将是直接引用条目的 URL。要确定`getName()`返回的名称是相对还是绝对，请使用[`isRelative()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#isRelative--)。如果此方法返回`true`，则名称相对于目标上下文；如果返回`false`，则名称是一个 URL。

如果名称是一个 URL，您需要使用该 URL，则可以将其传递给了解 URL 的初始上下文（请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/url.html)）。

如果您需要获取条目的完整 DN，可以使用[`NameClassPair.getNameInNamespace()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getNameInNamespace--)。

## 对象

如果搜索是请求返回条目对象的（使用[`SearchControls.setReturningObjFlag()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setReturningObjFlag-boolean-)调用为`true`），那么`SearchResult`将包含表示条目的对象。要检索此对象，您需要调用[`getObject()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Binding.html#getObject--)。如果之前将`java.io.Serializable`、[`Referenceable`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Referenceable.html)或[`Reference`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Reference.html)对象绑定到 LDAP 名称，则使用来自条目的属性来重建该对象（请参阅[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/objects/reading/search.html)中的示例）。否则，使用来自条目的属性创建代表 LDAP 条目的`DirContext`实例。在任一情况下，LDAP 提供程序会在对象上调用[`DirectoryManager.getObjectInstance()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/DirectoryManager.html#getObjectInstance-java.lang.Object-javax.naming.Name-javax.naming.Context-java.util.Hashtable-javax.naming.directory.Attributes-)并返回结果。

## 类名

如果搜索是请求返回条目对象，则类名是从返回的对象派生的。如果搜索请求包括检索 LDAP 条目的`"javaClassName"`属性，则类名是该属性的值。否则，类名为`"javax.naming.directory.DirContext"`。类名是从[`getClassName()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getClassName--)获取的。

## 属性

当执行搜索时，您可以通过向其中一个[`search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-java.lang.String:A-)方法提供参数或使用[`SearchControls.setReturningAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setReturningAttributes-java.lang.String:A-)设置搜索控件来选择返回属性。如果没有明确指定属性，则将返回所有 LDAP 条目的属性。要指定不返回任何属性，必须传递一个空数组（`new String[0]`）。

要检索 LDAP 条目的属性，您需要在`SearchResult`上调用[`getAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchResult.html#getAttributes--)。

## 响应控件

查看[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/response.html)中的“控件和扩展”课程，了解如何检索搜索结果的响应控件的详细信息。
