# 更多 LDAP 操作

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/rename.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/rename.html)

LDAP 课程的其余部分介绍了 JNDI 提供的执行某些有趣的 LDAP 操作的能力。

## 重命名对象

您可以使用[`Context.rename()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rename-javax.naming.Name-javax.naming.Name-)来重命名目录中的对象。在[LDAP v2](http://www.ietf.org/rfc/rfc1777.txt)中，这对应于“修改 RDN”操作，该操作将在同一上下文中重命名条目（即重命名同级）。在[LDAP v3](http://www.ietf.org/rfc/rfc2251.txt)中，这对应于“修改 DN”操作，类似于“修改 RDN”，只是旧条目和新条目不需要在同一上下文中。您可以使用`Context.rename()`来重命名叶条目或内部节点。在 Naming and Directory Operations 课程中展示的示例重命名了一个叶条目。以下`代码`将内部节点从`"ou=NewHires"`重命名为`"ou=OldHires"`：

```java
ctx.rename("ou=NewHires", "ou=OldHires");

```

* * *

**注意：** [Oracle Directory Server v5.2](http://www.oracle.com/technetwork/testcontent/index-085178.html) 不支持重命名内部节点。如果您运行此示例，则会收到[`ContextNotEmptyException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ContextNotEmptyException.html)。

* * *

### 将条目重命名为 DIT 的不同部分

使用 LDAP v3，您可以将条目重命名为 DIT 的不同部分。要通过`Context.rename()`实现此目的，必须使用一个既是新条目又是旧条目的共同祖先的上下文。例如，要将`"cn=C. User, ou=NewHires, o=JNDITutorial"`重命名为`"cn=C. User, ou=People, o=JNDITutorial"`，您必须使用由`"o=JNDITutorial"`命名的上下文。以下是演示此操作的`示例`。如果您尝试在 LDAP v2 服务器上运行此示例，则会收到[`InvalidNameException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/InvalidNameException.html)，因为版本 2 不支持此功能。

```java
ctx.rename("cn=C. User, ou=NewHires", "cn=C. User, ou=People");

```

* * *

**注意：** [Oracle Directory Server v5.2](http://www.oracle.com/technetwork/testcontent/index-085178.html) 不支持使用不同父节点进行重命名。如果您使用该服务器运行此示例，则会收到[`OperationNotSupportedException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/OperationNotSupportedException.html)（表示“协议错误”）。

* * *

### 保留旧名称属性

在 LDAP 中，当您重命名条目时，您可以选择将条目的旧 RDN 保留为更新后条目的属性。例如，如果您将条目`"cn=C. User"`重命名为`"cn=Claude User"`，您可以指定是否要保留旧 RDN`"cn=C. User"`作为属性。

要指定在使用`Context.rename()`时是否要保留旧名称属性，请使用`"java.naming.ldap.deleteRDN"`环境属性。如果此属性的值为`"true"`（默认值），则旧的 RDN 将被移除。如果其值为`"false"`，则旧的 RDN 将保留为更新条目的属性。完整示例在`这里`。

```java
// Set the property to keep RDN
env.put("java.naming.ldap.deleteRDN", "false");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// Perform the rename
ctx.rename("cn=C. User, ou=NewHires", "cn=Claude User,ou=NewHires");

```
