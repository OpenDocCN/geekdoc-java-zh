# 使用 DTDHandler 和 EntityResolver

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/sax/using.html`](https://docs.oracle.com/javase/tutorial/jaxp/sax/using.html)

本节介绍了另外两个 SAX 事件处理程序：`DTDHandler`和`EntityResolver`。当 DTD 遇到未解析的实体或符号声明时，将调用`DTDHandler`。当需要将 URN（公共 ID）解析为 URL（系统 ID）时，将使用`EntityResolver`。

## `DTDHandler` API

选择解析器实现展示了引用包含二进制数据（如图像文件）的文件的方法，使用 MIME 数据类型。这是最简单、最可扩展的机制。但是，为了与旧的 SGML 样式数据兼容，也可以定义未解析的实体。

`NDATA`关键字定义了一个未解析的实体：

`<!ENTITY myEntity SYSTEM "..URL.." NDATA gif>`

`NDATA`关键字表示此实体中的数据不是可解析的 XML 数据，而是使用其他符号的数据。在本例中，符号被命名为`gif`。然后 DTD 必须包含该符号的声明，类似于以下内容。

`<!NOTATION gif SYSTEM "..URL..">`

当解析器看到未解析的实体或符号声明时，除了将其传递给应用程序使用`DTDHandler`接口外，它不会对信息做任何处理。该接口定义了两个方法。

+   `notationDecl(String name, String publicId, String systemId)`

+   `unparsedEntityDecl(String name, String publicId, String systemId, String notationName`

`notationDecl`方法传递符号的名称和公共或系统标识符，或两者，取决于 DTD 中声明了哪个。`unparsedEntityDecl`方法传递实体的名称、适当的标识符和它使用的符号的名称。

* * *

**注意 -** `DTDHandler`接口由`DefaultHandler`类实现。

* * *

符号也可以用于属性声明。例如，以下声明需要 GIF 和 PNG 图像文件格式的符号。

```java
<!ENTITY image EMPTY>
<!ATTLIST image ...  type  NOTATION  (gif | png) "gif">

```

在这里，类型声明为 gif 或 png。如果没有指定，则默认为 gif。

无论符号引用用于描述未解析的实体还是属性，都由应用程序进行适当处理。解析器对符号的语义一无所知。它只传递声明。

## `EntityResolver` API

`EntityResolver` API 允许您将公共 ID（URN）转换为系统 ID（URL）。例如，您的应用程序可能需要将类似`href="urn:/someName"`的内容转换为`"http://someURL"`。

`EntityResolver`接口定义了一个方法：

`resolveEntity(String publicId, String systemId)`

这种方法返回一个`InputSource`对象，可以用来访问实体的内容。将 URL 转换为`InputSource`很容易。但作为系统 ID 传递的 URL 很可能是原始文档的位置，而这个位置很可能在网络上的某个地方。要访问本地副本（如果有的话），必须在系统的某处维护一个目录，将名称（公共 ID）映射到本地 URL。
