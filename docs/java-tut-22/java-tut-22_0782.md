# 使用 StAX

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/stax/using.html`](https://docs.oracle.com/javase/tutorial/jaxp/stax/using.html)

一般来说，StAX 程序员通过使用 `XMLInputFactory`、`XMLOutputFactory` 和 `XMLEventFactory` 类来创建 XML 流读取器、写入器和事件。通过在工厂上设置属性来进行配置，可以通过在工厂上使用 `setProperty` 方法将特定于实现的设置传递给底层实现。类似地，可以使用 `getProperty` 工厂方法查询特定于实现的设置。

下面描述了 `XMLInputFactory`、`XMLOutputFactory` 和 `XMLEventFactory` 类，然后讨论了资源分配、命名空间和属性管理、错误处理，最后使用游标和迭代器 API 读取和写入流。

## StAX 工厂类

StAX 工厂类。`XMLInputFactory`、`XMLOutputFactory` 和 `XMLEventFactory`，让您定义和配置 XML 流读取器、流写入器和事件类的实现实例。

### XMLInputFactory

`XMLInputFactory` 类允许您配置由工厂创建的 XML 流读取器处理器的实现实例。通过在类上调用 `newInstance` 方法来创建抽象类 `XMLInputFactory` 的新实例。然后使用静态方法 `XMLInputFactory.newInstance` 来创建新的工厂实例。

派生自 JAXP，`XMLInputFactory.newInstance` 方法通过以下查找过程确定要加载的特定 `XMLInputFactory` 实现类：

1.  使用 `javax.xml.stream.XMLInputFactory` 系统属性。

1.  使用 Java SE 平台的 Java Runtime Environment (JRE) 目录中的 `lib/xml.stream.properties` 文件。

1.  如果可用，使用 Services API 通过查找 JRE 中可用的 JAR 文件中的 `META-INF/services/javax.xml.stream.XMLInputFactory` 文件确定类名。

1.  使用平台默认的 `XMLInputFactory` 实例。

在获取适当的 `XMLInputFactory` 引用之后，应用程序可以使用工厂来配置和创建流实例。以下表格列出了 `XMLInputFactory` 支持的属性。详细列表请参阅 StAX 规范。

`javax.xml.stream.XMLInputFactory` 属性

| 属性 | 描述 |
| --- | --- |
| `isValidating` | 打开实现特定的验证。 |
| `isCoalescing` | **(必需)** 要求处理器合并相邻的字符数据。 |
| `isNamespaceAware` | 关闭命名空间支持。所有实现必须支持命名空间。对非命名空间感知文档的支持是可选的。 |
| `isReplacingEntityReferences` | **(必需)** 要求处理器用其替换值替换内部实体引用，并将其报告为字符或描述实体的事件集。 |
| `isSupportingExternalEntities` | **(必需)** 要求处理器解析外部解析实体。 |
| `reporter` | **(必需)** 设置并获取`XMLReporter`接口的实现。 |
| `resolver` | **(必需)** 设置并获取`XMLResolver`接口的实现。 |
| `allocator` | **(必需)** 设置并获取`XMLEventAllocator`接口的实现。 |

### XMLOutputFactory

通过在类上调用`newInstance`方法来创建抽象类`XMLOutputFactory`的新实例。然后使用静态方法`XMLOutputFactory.newInstance`来创建一个新的工厂实例。用于获取实例的算法与`XMLInputFactory`相同，但引用`javax.xml.stream.XMLOutputFactory`系统属性。

`XMLOutputFactory` 只支持一个属性，即`javax.xml.stream.isRepairingNamespaces`。此属性是必需的，其目的是创建默认前缀并将其与命名空间 URI 关联起来。有关更多信息，请参阅 StAX 规范。

### `XMLEventFactory`

通过在类上调用`newInstance`方法来创建抽象类`XMLEventFactory`的新实例。然后使用静态方法`XMLEventFactory.newInstance`来创建一个新的工厂实例。此工厂引用`javax.xml.stream.XMLEventFactory`属性来实例化工厂。用于获取实例的算法与`XMLInputFactory`和`XMLOutputFactory`相同，但引用`javax.xml.stream.XMLEventFactory`系统属性。 

`XMLEventFactory` 没有默认属性。

## 资源、命名空间和错误

StAX 规范处理资源解析、属性和命名空间，以及错误和异常，如下所述。

### 资源解析

`XMLResolver`接口提供了在 XML 处理期间解析资源的方法。应用程序在`XMLInputFactory`上设置接口，然后该工厂实例创建的所有处理器都设置该接口。

### 属性和命名空间

属性由 StAX 处理器使用游标接口中的查找方法和字符串以及迭代器接口中的`Attribute`和`Namespace`事件报告。请注意，命名空间被视为属性，尽管在游标和迭代器 API 中，命名空间与属性分开报告。还要注意，命名空间处理对于 StAX 处理器是可选的。有关命名空间绑定和可选命名空间处理的完整信息，请参阅 StAX 规范。

### 错误报告和异常处理

所有致命错误都通过`javax.xml.stream.XMLStreamException`接口报告。所有非致命错误和警告都使用`javax.xml.stream.XMLReporter`接口报告。

## 读取 XML 流

正如在本课程前面所描述的，使用 StAX 处理器读取 XML 流的方式——更重要的是，您得到的内容——取决于您是使用 StAX 游标 API 还是事件迭代器 API，这两个部分描述了如何使用这两个 API 读取 XML 流。

### 使用 XMLStreamReader

StAX 游标 API 中的`XMLStreamReader`接口只允许您以向前方向读取 XML 流或文档，每次只能读取信息集中的一个项目。以下方法可用于从流中提取数据或跳过不需要的事件：

+   获取属性的值

+   读取 XML 内容

+   确定一个元素是否有内容或为空

+   获取对属性集合的索引访问

+   获取对命名空间集合的索引访问

+   获取当前事件的名称（如果适用）

+   获取当前事件的内容（如果适用）

`XMLStreamReader`的实例在任何时候都有一个当前事件，其方法在其上操作。当您在流上创建一个`XMLStreamReader`实例时，初始当前事件是`START_DOCUMENT`状态。然后可以使用`XMLStreamReader.next`方法来跳到流中的下一个事件。

### 读取属性、属性和命名空间

`XMLStreamReader.next`方法加载流中下一个事件的属性。然后，您可以通过调用`XMLStreamReader.getLocalName`和`XMLStreamReader.getText`方法来访问这些属性。

当`XMLStreamReader`游标位于`StartElement`事件上时，它读取事件的名称和任何属性，包括命名空间。可以使用索引值访问事件的所有属性，并且还可以通过命名空间 URI 和本地名称查找。但请注意，只有当前`StartEvent`上声明的命名空间可用；之前声明的命名空间不会被保留，重新声明的命名空间也不会被移除。

### XMLStreamReader 方法

`XMLStreamReader`提供以下方法来检索有关命名空间和属性的信息：

```java
int getAttributeCount();
String getAttributeNamespace(int index);
String getAttributeLocalName(int index);
String getAttributePrefix(int index);
String getAttributeType(int index);
String getAttributeValue(int index);
String getAttributeValue(String namespaceUri, String localName);
boolean isAttributeSpecified(int index);

```

也可以使用三种额外的方法访问命名空间：

```java
int getNamespaceCount();
String getNamespacePrefix(int index);
String getNamespaceURI(int index);

```

### 实例化一个 XMLStreamReader

这个示例取自 StAX 规范，展示了如何实例化一个输入工厂，创建一个读取器，并遍历 XML 流的元素：

```java
XMLInputFactory f = XMLInputFactory.newInstance();
XMLStreamReader r = f.createXMLStreamReader( ... );
while(r.hasNext()) {
    r.next();
}

```

### 使用 XMLEventReader

StAX 事件迭代器 API 中的`XMLEventReader` API 提供了将 XML 流中的事件映射到可以自由重用的分配的事件对象的方法，并且 API 本身可以扩展以处理自定义事件。

`XMLEventReader`提供了四种方法来迭代解析 XML 流：

+   `next`：返回流中的下一个事件

+   `nextEvent`：返回下一个类型化的 XMLEvent

+   `hasNext`：如果流中有更多事件要处理，则返回 true

+   `peek`：返回事件但不迭代到下一个事件

例如，以下代码片段说明了`XMLEventReader`方法声明：

```java
package javax.xml.stream;
import java.util.Iterator;

public interface XMLEventReader extends Iterator {
    public Object next();
    public XMLEvent nextEvent() throws XMLStreamException;
    public boolean hasNext();
    public XMLEvent peek() throws XMLStreamException;
    // ...
}

```

要读取流上的所有事件然后打印它们，您可以使用以下方法：

```java
while(stream.hasNext()) {
    XMLEvent event = stream.nextEvent();
    System.out.print(event);
}

```

### 读取属性

您可以从其关联的`javax.xml.stream.StartElement`中访问属性，如下所示：

```java
public interface StartElement extends XMLEvent {
    public Attribute getAttributeByName(QName name);
    public Iterator getAttributes();
}

```

您可以使用`StartElement`接口上的`getAttributes`方法来使用在该`StartElement`上声明的所有属性的`Iterator`。

### 读取命名空间

与读取属性类似，命名空间是通过调用`StartElement`接口上的`getNamespaces`方法创建的`Iterator`来读取的。仅返回当前`StartElement`的命名空间，并且应用程序可以通过使用`StartElement.getNamespaceContext`来获取当前命名空间上下文。

## 写入 XML 流

StAX 是一个双向 API，游标和事件迭代器 API 都有自己的一套接口用于写入 XML 流。与读取流的接口一样，写入器 API 对于游标和事件迭代器之间存在显著差异。以下部分描述了如何使用这些 API 之一来写入 XML 流。

### 使用 XMLStreamWriter

StAX 游标 API 中的`XMLStreamWriter`接口允许应用程序写回到 XML 流或创建全新的流。XMLStreamWriter 具有让您执行以下操作的方法：

+   写入格式良好的 XML

+   刷新或关闭输出

+   写入限定名称

请注意，`XMLStreamWriter`实现不需要对输入执行格式良好性或有效性检查。虽然一些实现可能执行严格的错误检查，但其他可能不会。您实现的规则适用于`XMLOutputFactory`类中定义的属性。

使用`writeCharacters`方法转义字符，如`&`、`<`、`>`和`"`。绑定前缀可以通过传递前缀的实际值，使用`setPrefix`方法，或设置默认命名空间声明的属性来处理。

以下示例取自 StAX 规范，展示了如何实例化输出工厂，创建写入器并写入 XML 输出：

```java
XMLOutputFactory output = XMLOutputFactory.newInstance();
XMLStreamWriter writer = output.createXMLStreamWriter( ... );

writer.writeStartDocument();
writer.setPrefix("c","http://c");
writer.setDefaultNamespace("http://c");

writer.writeStartElement("http://c","a");
writer.writeAttribute("b","blah");
writer.writeNamespace("c","http://c");
writer.writeDefaultNamespace("http://c");

writer.setPrefix("d","http://c");
writer.writeEmptyElement("http://c","d");
writer.writeAttribute("http://c", "chris","fry");
writer.writeNamespace("d","http://c");
writer.writeCharacters("Jean Arp");
writer.writeEndElement();

writer.flush();

```

此代码生成以下 XML（新行不是规范性的）：

```java
<?xml version=’1.0’ encoding=’utf-8’?>
<a b="blah"  >
<d:d d:chris="fry" />Jean Arp</a>

```

### 使用 XMLEventWriter

StAX 事件迭代器 API 中的`XMLEventWriter`接口允许应用程序写回到 XML 流或创建全新的流。此 API 可以扩展，但主要 API 如下：

```java
public interface XMLEventWriter {
    public void flush() throws XMLStreamException;
    public void close() throws XMLStreamException;
    public void add(XMLEvent e) throws XMLStreamException;
    // ... other methods not shown.
}

```

`XMLEventWriter`的实例是由`XMLOutputFactory`的实例创建的。流事件被迭代地添加，一旦添加到事件写入器实例后，事件就不能被修改。

### 属性、转义字符、绑定前缀

StAX 实现需要缓冲最后一个`StartElement`，直到在流中添加或遇到除`Attribute`或`Namespace`之外的事件。这意味着当您向流中添加`Attribute`或`Namespace`时，它会附加到当前的`StartElement`事件。

您可以使用`Characters`方法转义字符如`&`、`<`、`>`和`"`。

`setPrefix(...)` 方法可用于显式绑定输出时使用的前缀，而 `getPrefix(...)` 方法可用于获取当前前缀。请注意，默认情况下，`XMLEventWriter` 会将命名空间绑定添加到其内部命名空间映射中。前缀在绑定它们的事件对应的 `EndElement` 后会失效。
