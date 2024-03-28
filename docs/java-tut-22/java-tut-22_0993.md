# 过滤器

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/filter.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/filter.html)

除了使用一组属性指定搜索外，还可以以*搜索过滤器*的形式指定搜索。搜索过滤器是以逻辑表达式形式表达的搜索查询。[`DirContext.search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-)接受的搜索过滤器语法在[RFC 2254](http://www.ietf.org/rfc/rfc2254.txt)中有描述。

以下搜索过滤器指定符合条件的条目必须具有值为`"Geisel"`的`"sn"`属性和任何值的`"mail"`属性：

```java
(&(sn=Geisel)(mail=*))

```

以下代码创建一个过滤器和默认的[`SearchControls`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html)，并使用它们执行搜索。该搜索等同于基本搜索示例中呈现的搜索。

```java
// Create the default search controls
SearchControls ctls = new SearchControls();

// Specify the search filter to match
// Ask for objects that have the attribute "sn" == "Geisel"
// and the "mail" attribute
String filter = "(&(sn=Geisel)(mail=*))";

// Search for objects using the filter
NamingEnumeration answer = ctx.search("ou=People", filter, ctls);

```

运行`此示例`会产生以下结果。

```java
# java SearchWithFilterRetAll
>>>cn=Ted Geisel
attribute: sn
value: Geisel
attribute: objectclass
value: top
value: person
value: organizationalPerson
value: inetOrgPerson
attribute: jpegphoto
value: [B@1dacd75e
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: facsimiletelephonenumber
value: +1 408 555 2329
attribute: cn
value: Ted Geisel
attribute: telephonenumber
value: +1 408 555 5252

```

## 搜索过滤器语法快速概述

搜索过滤器语法基本上是前缀表示法中的逻辑表达式（即，逻辑运算符出现在其参数之前）。以下表列出了用于创建过滤器的符号。

| 符号 | 描述 |
| --- | --- |
| & | 合取（即，*与* — 列表中的所有项目必须为真） |
| &#124; | 析取（即，*或* — 一个或多个备选项必须为真） |
| ! | 否定（即，*非* — 被否定的项目必须为假） |
| = | 等于（根据属性匹配规则） |
| ~= | 大致相等（根据属性匹配规则） |
| >= | 大于（根据属性匹配规则） |
| <= | 小于（根据属性匹配规则） |
| =* | 存在（即，条目必须具有属性，但其值无关紧要） |
| * | 通配符（表示该位置可以出现零个或多个字符）；用于指定要匹配的属性值时使用 |
| \ | 转义（用于在属性值中出现'*'、'('或')'时进行转义） |

每个过滤器中的项目都是使用属性标识符和属性值或表示属性值的符号组成的。例如，项目`"sn=Geisel"`表示`"sn"`属性必须具有属性值`"Geisel"`，而项目`"mail=*"`表示`"mail"`属性必须存在。

每个项目必须用一组括号括起来，如`"(sn=Geisel)"`。这些项目使用逻辑运算符（如"&"（合取））组合，以创建逻辑表达式，如`"(& (sn=Geisel) (mail=*))"`。

每个逻辑表达式都可以进一步由其他本身是逻辑表达式的项目组成，就像`"(| (& (sn=Geisel) (mail=*)) (sn=L*))"`中那样。最后一个示例请求具有`"sn"`属性为`"Geisel"`和`"mail"`属性的条目，或者其`"sn"`属性以字母"L"开头的条目。

有关语法的完整描述，请参阅[RFC 2254](http://ietf.org/rfc/rfc2254.txt)。

## 返回选定属性

前面的示例返回满足指定过滤器的条目关联的所有属性。您可以通过设置搜索控件参数来选择要返回的属性。您可以创建一个要包含在结果中的属性标识符数组，并将其传递给[`SearchControls.setReturningAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setReturningAttributes-java.lang.String:A-)。以下是一个示例。

```java
// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};
SearchControls ctls = new SearchControls();
ctls.setReturningAttributes(attrIDs);

```

这个示例等同于基本搜索部分中的返回选定属性示例。运行`这个示例`会产生以下结果。（该条目没有`"golfhandicap"`属性，因此不会返回。）

```java
# java SearchWithFilter
>>>cn=Ted Geisel
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```
