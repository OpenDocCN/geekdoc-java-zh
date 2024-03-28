# 修改属性

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/modattrs.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/modattrs.html)

[`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) 接口包含了修改目录中对象的属性和属性值的方法。

## 使用修改列表

修改对象属性的一种方法是提供修改请求的列表([`ModificationItem`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/ModificationItem.html))。每个`ModificationItem`包含一个数字常量，指示要进行的修改类型，以及描述要进行的修改的[`Attribute`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/Attribute.html)。以下是三种修改类型：

+   [添加属性](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#ADD_ATTRIBUTE)

+   [替换属性](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#REPLACE_ATTRIBUTE)

+   [删除属性](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#REMOVE_ATTRIBUTE)

修改按列表中出现的顺序应用。要么执行所有修改，要么一个也不执行。

以下代码创建了一个修改列表。它用值"geisel@wizards.com"替换了`"mail"`属性的值，向`"telephonenumber"`属性添加了一个额外的值，并删除了`"jpegphoto"`属性。

```java
// Specify the changes to make
ModificationItem[] mods = new ModificationItem[3];

// Replace the "mail" attribute with a new value
mods[0] = new ModificationItem(DirContext.REPLACE_ATTRIBUTE,
    new BasicAttribute("mail", "geisel@wizards.com"));

// Add an additional value to "telephonenumber"
mods[1] = new ModificationItem(DirContext.ADD_ATTRIBUTE,
    new BasicAttribute("telephonenumber", "+1 555 555 5555"));

// Remove the "jpegphoto" attribute
mods[2] = new ModificationItem(DirContext.REMOVE_ATTRIBUTE,
    new BasicAttribute("jpegphoto"));

```

* * *

**Windows Active Directory:** Active Directory 将"telephonenumber"定义为单值属性，与[RFC 2256](http://www.ietf.org/rfc/rfc2256.txt)相悖。要使此示例针对 Active Directory 正常工作，您必须使用除"telephonenumber"之外的属性，或将`DirContext.ADD_ATTRIBUTE`更改为`DirContext.REPLACE_ATTRIBUTE`。

* * *

创建此修改列表后，您可以将其提供给[`modifyAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#modifyAttributes-javax.naming.Name-javax.naming.directory.ModificationItem:A-) 如下所示。

```java
// Perform the requested modifications on the named object
ctx.modifyAttributes(name, mods);

```

## 使用属性

或者，您可以通过指定修改类型和要应用修改的属性来执行修改。

例如，以下行将用`orig`中的属性（在`name`中标识）替换为`orig`中的属性：

```java
ctx.modifyAttributes(name, DirContext.REPLACE_ATTRIBUTE, orig);

```

`name` 的任何其他属性保持不变。

`modifyAttributes()` 的这两种用法在`示例程序`中演示。该程序通过使用修改列表修改属性，然后使用`modifyAttributes()`的第二种形式来恢复原始属性。
