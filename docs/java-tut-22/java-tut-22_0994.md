# 范围

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/scope.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/scope.html)

默认的[`SearchControls`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html)指定搜索在命名上下文中执行（[`SearchControls.ONELEVEL_SCOPE`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#ONELEVEL_SCOPE)）。这个默认设置在搜索过滤器部分的示例中使用。

除了这个默认设置外，您还可以指定搜索在*整个子树*或仅在命名对象中执行。

## 搜索子树

对整个子树进行搜索将搜索命名对象及其所有后代。要使搜索以这种方式执行，请将[`SearchControls.SUBTREE_SCOPE`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#SUBTREE_SCOPE)传递给[`SearchControls.setSearchScope()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setSearchScope-int-)如下所示。

```java
// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};
SearchControls ctls = new SearchControls();
ctls.setReturningAttributes(attrIDs);
ctls.setSearchScope(SearchControls.SUBTREE_SCOPE);

// Specify the search filter to match
// Ask for objects that have the attribute "sn" == "Geisel"
// and the "mail" attribute
String filter = "(&(sn=Geisel)(mail=*))";

// Search the subtree for objects by using the filter
NamingEnumeration answer = ctx.search("", filter, ctls);

```

`这个示例` 搜索上下文`ctx`的子树，查找满足指定过滤器的条目。它在这个子树中找到了满足过滤器的条目`"cn= Ted Geisel, ou=People"`。

```java
# java SearchSubtree
>>>cn=Ted Geisel, ou=People
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```

## 搜索命名对象

你也可以搜索命名对象。例如，测试命名对象是否符合搜索过滤器非常有用。要搜索命名对象，请将[`SearchControls.OBJECT_SCOPE`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#OBJECT_SCOPE)传递给`setSearchScope()`。

```java
// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};
SearchControls ctls = new SearchControls();
ctls.setReturningAttributes(attrIDs);
ctls.setSearchScope(SearchControls.OBJECT_SCOPE);

// Specify the search filter to match
// Ask for objects that have the attribute "sn" == "Geisel"
// and the "mail" attribute
String filter = "(&(sn=Geisel)(mail=*))";

// Search the subtree for objects by using the filter
NamingEnumeration answer = 
    ctx.search("cn=Ted Geisel, ou=People", filter, ctls);

```

`这个示例` 测试对象`"cn=Ted Geisel, ou=People"`是否满足给定的过滤器。

```java
# java SearchObject
>>>
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```

该示例找到一个答案并将其打印出来。请注意，结果的名称是空字符串。这是因为对象的名称始终相对于搜索上下文命名（在本例中为`"cn=Ted Geisel, ou=People"`）。
