# StAX API

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/stax/api.html`](https://docs.oracle.com/javase/tutorial/jaxp/stax/api.html)

StAX API 公开了用于 XML 文档的迭代式、基于事件的处理的方法。XML 文档被视为一系列经过过滤的事件，并且信息集状态可以以过程化方式存储。此外，与 SAX 不同，StAX API 是双向的，可以实现对 XML 文档的读取和写入。

StAX API 实际上是两个不同的 API 集：一个**光标** API 和一个**迭代器** API。这两个 API 集将在本课程的后面更详细地解释，但它们的主要特点如下所述。

## 光标 API

如其名称所示，StAX **光标** API 表示一个光标，您可以使用它从头到尾遍历 XML 文档。这个光标一次只能指向一件事，并且总是向前移动，从不后退，通常一次移动一个信息集元素。

两个主要的光标接口是`XMLStreamReader`和`XMLStreamWriter`。`XMLStreamReader`包括了从 XML 信息模型中检索所有可能信息的访问方法，包括文档编码、元素名称、属性、命名空间、文本节点、起始标记、注释、处理指令、文档边界等等；例如：

```java
public interface XMLStreamReader {
    public int next() throws XMLStreamException;
    public boolean hasNext() throws XMLStreamException;

    public String getText();
    public String getLocalName();
    public String getNamespaceURI();
    // ... other methods not shown
}

```

您可以在`XMLStreamReader`上调用诸如`getText`和`getName`之类的方法，以获取当前光标位置的数据。`XMLStreamWriter`提供了与`StartElement`和`EndElement`事件类型对应的方法；例如：

```java
public interface XMLStreamWriter {
    public void writeStartElement(String localName) throws XMLStreamException;
    public void writeEndElement() throws XMLStreamException;
    public void writeCharacters(String text) throws XMLStreamException;
    // ... other methods not shown
}

```

光标 API 与 SAX 在许多方面相似。例如，可以直接访问字符串和字符信息的方法可用，并且可以使用整数索引访问属性和命名空间信息。与 SAX 一样，光标 API 方法将 XML 信息作为字符串返回，这减少了对象分配的需求。

## 迭代器 API

StAX **迭代器** API 将 XML 文档流表示为一组离散的事件对象。这些事件由应用程序拉取，并由解析器按照它们在源 XML 文档中读取的顺序提供。

基本的迭代器接口称为`XMLEvent`，并且为事件迭代器 API 表中列出的每种事件类型都有子接口。用于读取迭代器事件的主要解析器接口是`XMLEventReader`，用于写入迭代器事件的主要接口是`XMLEventWriter`。`XMLEventReader`接口包含五种方法，其中最重要的是`nextEvent`，它返回 XML 流中的下一个事件。`XMLEventReader`实现了`java.util.Iterator`，这意味着从`XMLEventReader`返回的内容可以被缓存或传递给可以与标准 Java 迭代器一起工作的程序；例如：

```java
public interface XMLEventReader extends Iterator {
    public XMLEvent nextEvent() throws XMLStreamException;
    public boolean hasNext();
    public XMLEvent peek() throws XMLStreamException;
    // ...
}

```

类似地，在迭代器 API 的输出端，你有：

```java
public interface XMLEventWriter {
    public void flush() throws XMLStreamException;
    public void close() throws XMLStreamException;
    public void add(XMLEvent e) throws XMLStreamException;
    public void add(Attribute attribute) throws XMLStreamException;
    // ...
}

```

### 迭代器事件类型

`XMLEvent`在事件迭代器 API 中定义的类型

| 事件类型 | 描述 |
| --- | --- |
| `StartDocument` | 报告一组 XML 事件的开始，包括编码、XML 版本和独立属性。 |
| `StartElement` | 报告元素的开始，包括任何属性和命名空间声明；还提供了开始标记的前缀、命名空间 URI 和本地名称的访问。 |
| `EndElement` | 报告元素的结束标记。如果已在相应的 `StartElement` 上显式设置了命名空间，则在此处可以调用已经超出范围的命名空间。 |
| `Characters` | 对应于 XML `CData` 部分和 `CharacterData` 实体。请注意，可忽略的空格和重要的空格也被报告为 `Character` 事件。 |
| `EntityReference` | 字符实体可以作为独立事件报告，应用程序开发人员可以选择解析或传递未解析的实体。默认情况下，实体会被解析。或者，如果不想将实体报告为事件，则可以替换文本并报告为 `Characters`。 |
| `ProcessingInstruction` | 报告底层处理指令的目标和数据。 |
| `Comment` | 返回注释的文本。 |
| `EndDocument` | 报告一组 XML 事件的结束。 |
| `DTD` | 报告与流相关联的（如果有的话）DTD 的信息，并提供一种返回在 DTD 中找到的自定义对象的方法。 |
| `Attribute` | 属性通常作为 `StartElement` 事件的一部分报告。然而，有时希望将属性作为独立的 `Attribute` 事件返回；例如，当命名空间作为 `XQuery` 或 `XPath` 表达式的结果返回时。 |
| `Namespace` | 与属性一样，命名空间通常作为 `StartElement` 的一部分报告，但有时希望将命名空间作为独立的 `Namespace` 事件报告。 |

请注意，只有在处理的文档包含 DTD 时，才会创建 `DTD`、`EntityDeclaration`、`EntityReference`、`NotationDeclaration` 和 `ProcessingInstruction` 事件。

### 事件映射示例

作为事件迭代器 API 如何映射 XML 流的示例，请考虑以下 XML 文档：

```java
<?xml version="1.0"?>
<BookCatalogue >
    <Book>
        <Title>Yogasana Vijnana: the Science of Yoga</Title>
        <ISBN>81-40-34319-4</ISBN>
        <Cost currency="INR">11.50</Cost>
    </Book>
</BookCatalogue>

```

此文档将被解析为十八个主要和次要事件，如下表所示。请注意，通常从主要事件而不是直接访问，可以访问用大括号（`{}`）显示的次要事件。

迭代器 API 事件映射示例

| # | 元素/属性 | 事件 |
| --- | --- | --- |
| 1 | `version="1.0"` | `StartDocument` |
| 2 | `isCData = false data = "\n" IsWhiteSpace = true` | `Characters` |
| 3 | `qname = BookCatalogue:http://www.publishing.org 属性 = null 命名空间 = {BookCatalogue" -> http://www.publishing.org"}` | `StartElement` |
| 4 | `qname = 书 属性 = null 命名空间 = null` | `StartElement` |
| 5 | `qname = 标题 属性 = null 命名空间 = null` | `StartElement` |
| 6 | `isCData = false data = "Yogasana Vijnana: the Science of Yoga\n\t" IsWhiteSpace = false` | `Characters` |
| 7 | `qname = Title namespaces = null` | `EndElement` |
| 8 | `qname = ISBN attributes = null namespaces = null` | `StartElement` |
| 9 | `isCData = false data = "81-40-34319-4\n\t" IsWhiteSpace = false` | `Characters` |
| 10 | `qname = ISBN namespaces = null` | `EndElement` |
| 11 | `qname = Cost attributes = {"currency" -> INR} namespaces = null` | `StartElement` |
| 12 | `isCData = false data = "11.50\n\t" IsWhiteSpace = false` | `Characters` |
| 13 | `qname = Cost namespaces = null` | `EndElement` |
| 14 | `isCData = false data = "\n" IsWhiteSpace = true` | `Characters` |
| 15 | `qname = Book namespaces = null` | `EndElement` |
| 16 | `isCData = false data = "\n" IsWhiteSpace = true` | `Characters` |
| 17 | `qname = BookCatalogue:http://www.publishing.org namespaces = {BookCatalogue" -> http://www.publishing.org"}` | `EndElement` |
| 18 |  | `EndDocument` |

在这个例子中有几个重要的事项需要注意：

+   事件按照文档中遇到相应的 XML 元素的顺序创建，包括元素的嵌套、打开和关闭元素、属性顺序、文档开始和文档结束等。

+   与正确的 XML 语法一样，所有容器元素都有相应的开始和结束事件；例如，每个 `StartElement` 都有一个对应的 `EndElement`，即使是空元素也是如此。

+   `Attribute` 事件被视为次要事件，并且可以从其对应的 `StartElement` 事件中访问。

+   与 `Attribute` 事件类似，`Namespace` 事件被视为次要事件，但在事件流中出现两次，并且可以从它们对应的 `StartElement` 和 `EndElement` 中分别访问两次。

+   所有元素都指定了 `Character` 事件，即使这些元素没有字符数据。同样，`Character` 事件可以跨事件分割。

+   StAX 解析器维护一个命名空间堆栈，其中保存了当前元素及其祖先元素定义的所有 XML 命名空间信息。通过 `javax.xml.namespace.NamespaceContext` 接口暴露的命名空间堆栈可以通过命名空间前缀或 URI 访问。

## 在游标和迭代器 API 之间进行选择

此时合理地问一下，“我应该选择哪个 API？我应该创建 `XMLStreamReader` 还是 `XMLEventReader` 的实例？为什么会有两种类型的 API？”

### 开发目标

StAX 规范的作者针对三种类型的开发者：

+   **图书馆和基础设施开发者**：创建应用服务器、JAXM、JAXB、JAX-RPC 等实现；需要高效、低级别的 API，并且具有最小的可扩展性要求。

+   **Java ME 开发者**：需要小型、简单的拉取解析库，并且具有最小的可扩展性需求。

+   **Java 平台企业版（Java EE）和 Java 平台标准版（Java SE）开发人员**：需要干净、高效的拉取解析库，同时需要灵活性来读取和写入 XML 流，创建新的事件类型，并扩展 XML 文档元素和属性。

鉴于这些广泛的开发类别，StAX 的作者认为定义两个小型、高效的 API 比过载一个更大、必然更复杂的 API 更有用。

### 比较游标和迭代器 API

在选择游标和迭代器 API 之间之前，你应该注意一些你可以使用迭代器 API 而不能使用游标 API 的事项：

+   从`XMLEvent`子类创建的对象是不可变的，可以在数组、列表和映射中使用，并且可以在解析器继续处理后传递到你的应用程序中。

+   你可以创建`XMLEvent`的子类型，这些子类型可以是全新的信息项，也可以是现有项目的扩展，但具有额外的方法。

+   你可以以比游标 API 更简单的方式向 XML 事件流中添加和删除事件。

同样，在做出选择时，请记住一些一般性建议：

+   如果你正在为特别受内存限制的环境编程，比如 Java ME，你可以使用游标 API 创建更小、更高效的代码。

+   如果性能是你的最高优先级——例如，在创建低级库或基础设施时——游标 API 更有效率。

+   如果你想创建 XML 处理管道，请使用迭代器 API。

+   如果你想修改事件流，请使用迭代器 API。

+   如果你希望你的应用程序能够处理事件流的可插拔处理，请使用迭代器 API。

+   一般来说，如果你没有明确偏好，建议使用迭代器 API，因为它更灵活、可扩展，从而“未雨绸缪”你的应用程序。
