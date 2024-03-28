# 课程：命名和目录操作

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/index.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/index.html)

您可以使用 JNDI 执行命名操作，包括读取操作和更新命名空间的操作。本课程中描述了以下操作：

+   查找对象

+   列出上下文的内容

+   添加、覆盖和删除绑定

+   重命名对象

+   创建和销毁子上下文

## 配置

在执行任何命名或目录服务操作之前，您需要获取一个*初始上下文*--进入命名空间的起始点。这是因为所有命名和目录服务上的方法都是相对于某个上下文执行的。要获取初始上下文，您必须按照以下步骤进行。

1.  选择要访问的相应服务的服务提供者。

1.  指定初始上下文需要的任何配置。

1.  调用[`InitialContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html#constructor_detail)构造函数。

## 步骤 1：选择初始上下文的服务提供者

您可以通过创建一组*环境属性*（`Hashtable`）并将服务提供者类的名称添加到其中来指定初始上下文要使用的服务提供者。环境属性在[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/beyond/env/index.html)中有详细描述。

如果您正在使用 JDK 中包含的 LDAP 服务提供者，则您的代码将如下所示。

```java
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "com.sun.jndi.ldap.LdapCtxFactory");

```

要在 JDK 中指定文件系统服务提供者，您需要编写类似以下代码的代码。

```java
Hashtable<String, Object> env = new Hashtable>String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "com.sun.jndi.fscontext.RefFSContextFactory");

```

您还可以使用*系统属性*来指定要使用的服务提供者。查看[JNDI 教程](https://docs.oracle.com/javase/jndi/tutorial/beyond/index.html)获取详细信息。

## 步骤 2：提供初始上下文所需的信息

不同目录的客户端可能需要各种信息来联系目录。例如，您可能需要指定服务器在哪台机器上运行以及需要什么信息来识别用户到目录。此类信息通过环境属性传递给服务提供者。JNDI 指定了一些通用的环境属性，服务提供者可以使用。您的服务提供者文档将详细说明这些属性所需的信息。

LDAP 提供程序要求程序指定 LDAP 服务器的位置，以及用户身份信息。为了提供这些信息，您需要编写如下代码。

```java
env.put(Context.PROVIDER_URL, "ldap://ldap.wiz.com:389");
env.put(Context.SECURITY_PRINCIPAL, "joeuser");
env.put(Context.SECURITY_CREDENTIALS, "joepassword");

```

本教程使用 JDK 中的 LDAP 服务提供程序。示例假设在本地机器上的端口 389 上设置了一个服务器，根专有名称为`"o=JNDITutorial"`，并且更新目录不需要认证。它们包括以下代码来设置环境。

```java
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

```

如果你正在使用设置不同的目录，那么你需要相应地设置这些环境属性。你需要将`"localhost"`替换为那台机器的名称。你可以运行这些示例代码来访问任何公共目录服务器或者运行在不同机器上的你自己的服务器。你需要将`"localhost"`替换为那台机器的名称，并将`o=JNDITutorial`替换为可用的命名上下文。

## 第三步：创建初始上下文

现在你已经准备好创建初始上下文了。为此，你需要将之前创建的环境属性传递给[`InitialContext`构造函数](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html#constructor_detail)：

```java
Context ctx = new InitialContext(env);

```

现在你已经有了一个指向[`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html)对象的引用，你可以开始访问命名服务。

要执行目录操作，你需要使用一个[`InitialDirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html)。为此，使用其中的一个[构造函数](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html#constructor_detail)：

```java
DirContext ctx = new InitialDirContext(env);

```

这个语句返回一个用于执行目录操作的[`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html)对象的引用。
