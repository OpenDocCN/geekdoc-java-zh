# LDAP 比较

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/compare.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/compare.html)

LDAP 的“比较”操作允许客户端询问服务器是否具有指定条目的属性/值对。这使得服务器可以保留某些属性/值对的机密性（即，不对一般“搜索”访问公开），同时仍允许客户端有限使用它们。例如，一些服务器可能会将此功能用于密码，尽管客户端在“比较”操作本身中传递明文密码是不安全的。

要在 JNDI 中实现这一点，请对以下方法使用适当限制的参数：

+   [`search(Name name, String filter, SearchControls ctls)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-)

+   [`search(Name name, String filterExpr, Object[]filterArgs, SearchControls ctls)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-java.lang.Object:A-javax.naming.directory.SearchControls-)

1.  过滤器必须是“(*name*=*value*)”的形式。不能使用通配符。

1.  搜索范围必须是[`SearchControls.OBJECT_SCOPE`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#OBJECT_SCOPE)。

1.  你必须要求不返回任何属性。如果不符合这些条件，那么这些方法将使用 LDAP 的“搜索”操作而不是 LDAP 的“比较”操作。

这里有一个`示例`，会导致使用 LDAP 的“比较”操作。

```java
// Value of the attribute
byte[] key = {(byte)0x61, (byte)0x62, (byte)0x63, (byte)0x64, 
              (byte)0x65, (byte)0x66, (byte)0x67};

// Set up the search controls
SearchControls ctls = new SearchControls();
ctls.setReturningAttributes(new String[0]);       // Return no attrs
ctls.setSearchScope(SearchControls.OBJECT_SCOPE); // Search object only

// Invoke search method that will use the LDAP "compare" operation
NamingEnumeration answer = ctx.search("cn=S. User, ou=NewHires", 
                                      "(mySpecialKey={0})", 
                                       new Object[]{key}, ctls);

```

如果比较成功，结果枚举将包含一个名称为空且不包含任何属性的单个项目。
