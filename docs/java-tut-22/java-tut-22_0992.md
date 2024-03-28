# 基本搜索

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/basicsearch.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/basicsearch.html)

最简单的搜索形式要求您指定条目必须具有的属性集，以及执行搜索的目标上下文的名称。

以下代码创建了一个属性集`matchAttrs`，其中包含两个属性`"sn"`和`"mail"`。 它指定符合条件的条目必须具有一个姓氏（`"sn"`）属性，其值为`"Geisel"`，以及一个任何值的`"mail"`属性。 然后调用[`DirContext.search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-)在上下文`"ou=People"`中搜索具有`matchAttrs`指定属性的条目。

```java
// Specify the attributes to match
// Ask for objects that has a surname ("sn") attribute with 
// the value "Geisel" and the "mail" attribute

// ignore attribute name case
Attributes matchAttrs = new BasicAttributes(true); 
matchAttrs.put(new BasicAttribute("sn", "Geisel"));
matchAttrs.put(new BasicAttribute("mail"));

// Search for objects that have those matching attributes
NamingEnumeration answer = ctx.search("ou=People", matchAttrs);

```

你可以按照以下方式打印结果。

```java
while (answer.hasMore()) {
    SearchResult sr = (SearchResult)answer.next();
    System.out.println(">>>" + sr.getName());
    printAttrs(sr.getAttributes());
}

```

`printAttrs()`类似于`getAttributes()`示例中打印属性集的代码。

运行`这个例子`会产生以下结果。

```java
# java SearchRetAll
>>>cn=Ted Geisel
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
attribute: cn
value: Ted Geisel
attribute: telephonenumber
value: +1 408 555 5252

```

## 返回选定的属性

前面的例子返回满足指定查询条件的条目关联的所有属性。 您可以通过向`search()`传递要包含在结果中的属性标识符数组来选择要返回的属性。 在之前显示的创建`matchAttrs`之后，您还需要创建属性标识符数组，如下所示。

```java
// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};

// Search for objects that have those matching attributes
NamingEnumeration answer = ctx.search("ou=People", matchAttrs, attrIDs);

```

`这个例子`返回具有属性`"sn"`、`"telephonenumber"`、`"golfhandicap"`和`"mail"`的条目，这些条目具有一个属性`"mail"`，并且具有一个值为`"Geisel"`的`"sn"`属性。 这个例子产生以下结果。（该条目没有`"golfhandicap"`属性，因此不返回。）

```java
# java Search 
>>>cn=Ted Geisel
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```
