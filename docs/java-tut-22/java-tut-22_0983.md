# 列出上下文

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/list.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/list.html)

与[`Context.lookup()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-)一次获取一个对象不同，您可以通过一次操作列出整个上下文。有两种列出上下文的方法：一种返回绑定，另一种仅返回名称到对象类名对。

## Context.List()方法

[`Context.list()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#list-javax.naming.Name-)返回一个[`NameClassPair`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html)的枚举。每个`NameClassPair`包含对象的名称和类名。以下代码片段列出了`"ou=People"`目录的内容（即在`"ou=People"`目录中找到的文件和目录）。

```java
NamingEnumeration list = ctx.list("ou=People");

while (list.hasMore()) {
    NameClassPair nc = (NameClassPair)list.next();
    System.out.println(nc);
}

```

运行`这个示例`会产生以下输出。

```java
# java List
cn=Jon Ruiz: javax.naming.directory.DirContext
cn=Scott Seligman: javax.naming.directory.DirContext
cn=Samuel Clemens: javax.naming.directory.DirContext
cn=Rosanna Lee: javax.naming.directory.DirContext
cn=Maxine Erlund: javax.naming.directory.DirContext
cn=Niels Bohr: javax.naming.directory.DirContext
cn=Uri Geller: javax.naming.directory.DirContext
cn=Colleen Sullivan: javax.naming.directory.DirContext
cn=Vinnie Ryan: javax.naming.directory.DirContext
cn=Rod Serling: javax.naming.directory.DirContext
cn=Jonathan Wood: javax.naming.directory.DirContext
cn=Aravindan Ranganathan: javax.naming.directory.DirContext
cn=Ian Anderson: javax.naming.directory.DirContext
cn=Lao Tzu: javax.naming.directory.DirContext
cn=Don Knuth: javax.naming.directory.DirContext
cn=Roger Waters: javax.naming.directory.DirContext
cn=Ben Dubin: javax.naming.directory.DirContext
cn=Spuds Mackenzie: javax.naming.directory.DirContext
cn=John Fowler: javax.naming.directory.DirContext
cn=Londo Mollari: javax.naming.directory.DirContext
cn=Ted Geisel: javax.naming.directory.DirContext

```

## Context.listBindings()方法

[`Context.listBindings()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#listBindings-javax.naming.Name-)返回一个[`Binding`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Binding.html)的枚举。`Binding`是`NameClassPair`的子类。一个绑定不仅包含对象的名称和类名，还包含对象本身。以下代码枚举了`"ou=People"`上下文，打印出每个绑定的名称和对象。

```java
NamingEnumeration bindings = ctx.listBindings("ou=People");

while (bindings.hasMore()) {
    Binding bd = (Binding)bindings.next();
    System.out.println(bd.getName() + ": " + bd.getObject());
}

```

运行`这个示例`会产生以下输出。

```java
# java ListBindings
cn=Jon Ruiz: com.sun.jndi.ldap.LdapCtx@1d4c61c
cn=Scott Seligman: com.sun.jndi.ldap.LdapCtx@1a626f
cn=Samuel Clemens: com.sun.jndi.ldap.LdapCtx@34a1fc
cn=Rosanna Lee: com.sun.jndi.ldap.LdapCtx@176c74b
cn=Maxine Erlund: com.sun.jndi.ldap.LdapCtx@11b9fb1
cn=Niels Bohr: com.sun.jndi.ldap.LdapCtx@913fe2
cn=Uri Geller: com.sun.jndi.ldap.LdapCtx@12558d6
cn=Colleen Sullivan: com.sun.jndi.ldap.LdapCtx@eb7859
cn=Vinnie Ryan: com.sun.jndi.ldap.LdapCtx@12a54f9
cn=Rod Serling: com.sun.jndi.ldap.LdapCtx@30e280
cn=Jonathan Wood: com.sun.jndi.ldap.LdapCtx@16672d6
cn=Aravindan Ranganathan: com.sun.jndi.ldap.LdapCtx@fd54d6
cn=Ian Anderson: com.sun.jndi.ldap.LdapCtx@1415de6
cn=Lao Tzu: com.sun.jndi.ldap.LdapCtx@7bd9f2
cn=Don Knuth: com.sun.jndi.ldap.LdapCtx@121cc40
cn=Roger Waters: com.sun.jndi.ldap.LdapCtx@443226
cn=Ben Dubin: com.sun.jndi.ldap.LdapCtx@1386000
cn=Spuds Mackenzie: com.sun.jndi.ldap.LdapCtx@26d4f1
cn=John Fowler: com.sun.jndi.ldap.LdapCtx@1662dc8
cn=Londo Mollari: com.sun.jndi.ldap.LdapCtx@147c5fc
cn=Ted Geisel: com.sun.jndi.ldap.LdapCtx@3eca90

```

## 终止 NamingEnumeration

一个[`NamingEnumeration`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html)可以以自然、显式或意外的方式终止。

+   当[`NamingEnumeration.hasMore()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html#hasMore--)返回`false`时，枚举已完成并实际上被终止。

+   您可以通过调用[`NamingEnumeration.close()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html#close--)在枚举完成之前显式终止枚举。这样做会向底层实现提供一个提示，释放与枚举相关的任何资源。

+   如果`hasMore()`或[`next()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html#next--)抛出[`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html)，则枚举实际上被终止。

无论枚举如何被终止，一旦终止，就不能再使用。在终止的枚举上调用方法会产生未定义的结果。

## 为什么有两种不同的列出方法？

`list()`适用于浏览器样式的应用程序，只需显示上下文中对象的名称。例如，浏览器可能会列出上下文中的名称，并等待用户选择其中一个或几个名称以执行进一步操作。这类应用程序通常不需要访问上下文中的所有对象。

`listBindings()` 适用于需要对上下文中的对象进行批量操作的应用程序。例如，备份应用程序可能需要对文件目录中的所有对象执行“文件统计”操作。或者打印机管理程序可能希望重新启动建筑物中的所有打印机。为了执行这些操作，这些应用程序需要获取上下文中绑定的所有对象。因此，将对象作为枚举的一部分返回更为方便。

应用程序可以使用`list()`或可能更昂贵的`listBindings()`，具体取决于它所需的信息类型。
