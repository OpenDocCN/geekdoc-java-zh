# 读取属性

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/getattrs.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/getattrs.html)

要从目录中读取对象的属性，请使用[`DirContext.getAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#getAttributes-javax.naming.Name-)并传递您想要属性的对象的名称。假设命名服务中的一个对象的名称为`"cn=Ted Geisel, ou=People"`。要检索此对象的属性，您需要类似于以下的`code`：

```java
Attributes answer = ctx.getAttributes("cn=Ted Geisel, ou=People");

```

您可以按照以下方式打印此答案的内容。

```java
for (NamingEnumeration ae = answer.getAll(); ae.hasMore();) {
    Attribute attr = (Attribute)ae.next();
    System.out.println("attribute: " + attr.getID());
    /* Print each value */
    for (NamingEnumeration e = attr.getAll(); e.hasMore();
         System.out.println("value: " + e.next()))
        ;
}

```

这将产生以下输出。

```java
# java GetattrsAll
attribute: sn
value: Geisel
attribute: objectclass
value: top
value: person
value: organizationalPerson
value: inetOrgPerson
attribute: jpegphoto
value: [B@1dacd78b
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: facsimiletelephonenumber
value: +1 408 555 2329
attribute: telephonenumber
value: +1 408 555 5252
attribute: cn
value: Ted Geisel

```

## 返回选定的属性

要读取属性的选择性子集，您需要提供一个字符串数组，这些字符串是您想要检索的属性的属性标识符。

```java
// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};

// Get the attributes requested
Attributes answer = ctx.getAttributes("cn=Ted Geisel, ou=People", attrIDs);

```

`此示例`请求对象`"cn=Ted Geisel, ou=People"`的`"sn"`、`"telephonenumber"`、`"golfhandicap"`和`"mail"`属性。此对象除了`"golfhandicap"`属性外，其余属性都存在，因此答案中返回了三个属性。以下是示例的输出。

```java
# java Getattrs
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```
