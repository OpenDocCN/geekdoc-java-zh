# 处理词法事件

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/sax/events.html`](https://docs.oracle.com/javase/tutorial/jaxp/sax/events.html)

到目前为止，您已经消化了许多 XML 概念，包括 DTD 和外部实体。您还学会了如何使用 SAX 解析器。本课程的其余部分涵盖了您只有在编写基于 SAX 的应用程序时才需要理解的高级主题。如果您的主要目标是编写基于 DOM 的应用程序，您可以直接跳转到文档对象模型。

您之前看到，如果您将文本写出为 XML，您需要知道是否处于 CDATA 部分中。如果是，则尖括号（<）和和号（&）应保持不变输出。但如果不在 CDATA 部分中，则应将它们替换为预定义的实体`&lt;`和`&amp;`。但是您如何知道自己是否在处理 CDATA 部分？

另一方面，如果您以某种方式过滤 XML，您希望传递注释。通常解析器会忽略注释。您如何获取注释以便可以回显它们？

本节回答了这些问题。它向您展示了如何使用`org.xml.sax.ext.LexicalHandler`来识别注释、CDATA 部分和对解析实体的引用。

注释、CDATA 标记和对解析实体的引用构成词法信息-即，涉及 XML 文本本身而不是 XML 信息内容的信息。当然，大多数应用程序只关注 XML 文档的内容。这些应用程序将不使用`LexicalEventListener` API。但是输出 XML 文本的应用程序会发现它非常有价值。

* * *

**注意 -** 词法事件处理是一个可选的解析器功能。解析器实现不需要支持它。（参考实现是这样的。）本讨论假定您的解析器支持它。

* * *

## `LexicalHandler`的工作原理

要在 SAX 解析器看到词法信息时得到通知，您需要使用`LexicalHandler`配置解析器底层的`XmlReader`。`LexicalHandler`接口定义了以下事件处理方法。

`comment(String comment)`

将注释传递给应用程序。

`startCDATA()`, `endCDATA()`

告诉您 CDATA 部分何时开始和结束，这告诉您的应用程序下次调用`characters()`时可以期望什么样的字符。

`startEntity(String name)`, `endEntity(String name)`

给出解析实体的名称。

`startDTD(String name, String publicId, String systemId)`, `endDTD()`

告诉您正在处理 DTD，并标识它。

要激活词法处理程序，您的应用程序必须扩展`DefaultHandler`并实现`LexicalHandler`接口。然后，您必须配置您的`XMLReader`实例，使解析器委托给它，并配置它将词法事件发送到您的词法处理程序，如下所示。

```java
// ...

SAXParser saxParser = factory.newSAXParser();
XMLReader xmlReader = saxParser.getXMLReader();
xmlReader.setProperty("http://xml.org/sax/properties/lexical-handler",
                      handler); 
// ...

```

在这里，您可以使用`XMLReader`类中定义的`setProperty()`方法来配置`XMLReader`。作为 SAX 标准的一部分定义的属性名称是 URN，`http://xml.org/sax/properties/lexical-handler`。

最后，添加类似以下代码来定义将实现接口的适当方法。

```java
// ...

public void warning(SAXParseException err) {
    // ...
}

public void comment(char[] ch, int start, int length) throws SAXException {
    // ...   
}

public void startCDATA() throws SAXException {
    // ...
}

public void endCDATA() throws SAXException {
    // ...
}

public void startEntity(String name) throws SAXException {
    // ...
}

public void endEntity(String name) throws SAXException {
    // ...
}

public void startDTD(String name, String publicId, String systemId)
    throws SAXException {
    // ...
}

public void endDTD() throws SAXException {
    // ...
}

private void echoText() {
    // ...
}

// ...

```

这段代码将把您的解析应用程序转换为一个词法处理程序。剩下的就是为这些新方法中的每一个指定一个要执行的操作。
