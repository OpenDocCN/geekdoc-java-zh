# 检索 Distinguished Name

> 原文：[`docs.oracle.com/javase/tutorial/jndi/newstuff/dn.html`](https://docs.oracle.com/javase/tutorial/jndi/newstuff/dn.html)

在 JDK 5.0 之前的版本中，没有直接的方法从搜索结果中获取 Distinguished Name (DN)。`SearchResults.getName()` 方法始终返回相对于执行搜索的上下文的名称。为了获取搜索条目的绝对或完整名称，需要一定的记录来跟踪祖先上下文。在 JDK 5.0 中添加了以下两个新的 API，用于在对上下文执行搜索、列出或列出绑定操作时从 NameClassPair 中检索绝对名称：

+   [`NameClassPair.getNameInNameSpace(Name name)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getNameInNamespace-Name-)

+   [`NameClassPair.getNameInNameSpace(String name)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getNameInNamespace-String-)

这是一个从 LDAP 搜索中检索 DN 的示例：

```java
     public static void printSearchEnumeration(NamingEnumeration retEnum) {
         try {
             while (retEnum.hasMore()) {
                 SearchResult sr = (SearchResult) retEnum.next();
                 System.out.println(">>" + sr.getNameInNamespace());
             }
         } catch (NamingException e) {
             e.printStackTrace();
         }
     }

```

完整的示例可以从`这里`获取。该程序生成以下输出：

```java
        >>cn=Jon Ruiz, ou=People, o=JNDITutorial
        >>cn=Scott Seligman, ou=People, o=JNDITutorial
        >>cn=Samuel Clemens, ou=People, o=JNDITutorial
        >>cn=Rosanna Lee, ou=People, o=JNDITutorial
        >>cn=Maxine Erlund, ou=People, o=JNDITutorial
        >>cn=Niels Bohr, ou=People, o=JNDITutorial
        >>cn=Uri Geller, ou=People, o=JNDITutorial
        >>cn=Colleen Sullivan, ou=People, o=JNDITutorial
        >>cn=Vinnie Ryan, ou=People, o=JNDITutorial
        >>cn=Rod Serling, ou=People, o=JNDITutorial
        >>cn=Jonathan Wood, ou=People, o=JNDITutorial
        >>cn=Aravindan Ranganathan, ou=People, o=JNDITutorial
        >>cn=Ian Anderson, ou=People, o=JNDITutorial
        >>cn=Lao Tzu, ou=People, o=JNDITutorial
        >>cn=Don Knuth, ou=People, o=JNDITutorial
        >>cn=Roger Waters, ou=People, o=JNDITutorial
        >>cn=Ben Dubin, ou=People, o=JNDITutorial
        >>cn=Spuds Mackenzie, ou=People, o=JNDITutorial
        >>cn=John Fowler, ou=People, o=JNDITutorial
        >>cn=Londo Mollari, ou=People, o=JNDITutorial
        >>cn=Ted Geisel, ou=People,o=JNDITutorial 

```
