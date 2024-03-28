# 示例代码

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/stax/example.html`](https://docs.oracle.com/javase/tutorial/jaxp/stax/example.html)

本节逐步介绍了 JAXP 参考实现包中包含的示例 StAX 代码。本节中使用的所有示例目录均位于*INSTALL_DIR*`/jaxp-`*version*`/samples/stax`目录中。

本节涵盖的主题如下：

+   示例代码组织

+   示例 XML 文档

+   游标示例

+   游标到事件示例

+   事件示例

+   过滤器示例

+   读写示例

+   写入示例

## 示例代码组织

*INSTALL_DIR*`/jaxp-`*version*`/samples/stax`目录包含六个 StAX 示例目录：

+   **游标示例**：`cursor`目录包含`CursorParse.java`，演示如何使用`XMLStreamReader`（游标）API 读取 XML 文件。

+   **游标到事件示例**：`cursor2event`目录包含`CursorApproachEventObject.java`，演示应用程序如何在使用游标 API 时将信息作为`XMLEvent`对象获取。

+   **事件示例**：`event`目录包含`EventParse.java`，演示如何使用`XMLEventReader`（事件迭代器）API 读取 XML 文件。

+   **过滤器示例**：`filter`目录包含`MyStreamFilter.java`，演示如何使用 StAX 流过滤器 API。在此示例中，过滤器仅接受`StartElement`和`EndElement`事件，并过滤掉其余事件。

+   **读写示例**：`readnwrite`目录包含`EventProducerConsumer.java`，演示了如何使用 StAX 生产者/消费者机制同时读取和写入 XML 流。

+   **写入示例**：`writer`目录包含`CursorWriter.java`，演示如何使用`XMLStreamWriter`以编程方式编写 XML 文件。

除了写入示例外，本节中的所有 StAX 示例均使用示例 XML 文档`BookCatalog.xml`。

## 示例 XML 文档

大多数 StAX 示例类使用的示例 XML 文档`BookCatalog.xml`是一个基于常见`BookCatalogue`命名空间的简单图书目录。`BookCatalog.xml`的内容如下：

```java
<?xml version="1.0" encoding="UTF-8"?>
<BookCatalogue >
<Book>
    <Title>Yogasana Vijnana: the Science of Yoga</Title>
    <author>Dhirendra Brahmachari</Author>
    <Date>1966</Date>
    <ISBN>81-40-34319-4</ISBN>
    <Publisher>Dhirendra Yoga Publications</Publisher>
    <Cost currency="INR">11.50</Cost>
</Book>
<Book>
    <Title>The First and Last Freedom</Title>
    <Author>J. Krishnamurti</Author>
    <Date>1954</Date>
    <ISBN>0-06-064831-7</ISBN>
    <Publisher>Harper &amp; Row</Publisher>
    <Cost currency="USD">2.95</Cost>
</Book>
</BookCatalogue>

```

## 游标示例

位于*INSTALL_DIR*`/jaxp-`*version*`/samples/stax/cursor/`目录中，`CursorParse.java`演示了如何使用 StAX 游标 API 读取 XML 文档。在游标示例中，应用程序通过调用`next()`指示解析器读取 XML 输入流中的下一个事件。

请注意，`next()`只返回与解析器所处位置对应的整数常量。应用程序需要调用相关函数以获取与底层事件相关的更多信息。

您可以将这种方法想象成虚拟游标在 XML 输入流中移动。当虚拟游标位于特定事件时，可以调用各种访问器方法。

### 逐个事件地进行步进

在这个示例中，客户端应用程序通过在解析器上调用`next`方法来拉取 XML 流中的下一个事件；例如：

```java
try {
    for (int i = 0 ; i < count ; i++) {
        // pass the file name.. all relative entity
        // references will be resolved against this 
        // as base URI.
        XMLStreamReader xmlr = xmlif.createXMLStreamReader(filename,
                                   new FileInputStream(filename));

        // when XMLStreamReader is created, 
        // it is positioned at START_DOCUMENT event.
        int eventType = xmlr.getEventType();
        printEventType(eventType);
        printStartDocument(xmlr);

        // check if there are more events 
        // in the input stream
        while(xmlr.hasNext()) {
            eventType = xmlr.next();
            printEventType(eventType);

            // these functions print the information 
            // about the particular event by calling 
            // the relevant function
            printStartElement(xmlr);
            printEndElement(xmlr);
            printText(xmlr);
            printPIData(xmlr);
            printComment(xmlr);
        }
    }
}

```

请注意，`next`只是返回与当前游标位置下的事件对应的整数常量。应用程序调用相关函数以获取与底层事件相关的更多信息。当游标位于特定事件时，可以调用各种访问器方法。

### 返回字符串表示形式

因为`next`方法只返回与底层事件类型对应的整数，通常需要将这些整数映射到事件的字符串表示形式；例如：

```java
public final static String getEventTypeString(int eventType) {
    switch (eventType) {
        case XMLEvent.START_ELEMENT:
            return "START_ELEMENT";

        case XMLEvent.END_ELEMENT:
            return "END_ELEMENT";

        case XMLEvent.PROCESSING_INSTRUCTION:
            return "PROCESSING_INSTRUCTION";

        case XMLEvent.CHARACTERS:
            return "CHARACTERS";

        case XMLEvent.COMMENT:
            return "COMMENT";

        case XMLEvent.START_DOCUMENT:
            return "START_DOCUMENT";

        case XMLEvent.END_DOCUMENT:
            return "END_DOCUMENT";

        case XMLEvent.ENTITY_REFERENCE:
            return "ENTITY_REFERENCE";

        case XMLEvent.ATTRIBUTE:
            return "ATTRIBUTE";

        case XMLEvent.DTD:
            return "DTD";

        case XMLEvent.CDATA:
            return "CDATA";

        case XMLEvent.SPACE:
            return "SPACE";
    }
    return "UNKNOWN_EVENT_TYPE , " + eventType;
}

```

### 运行游标示例

1.  **要编译和运行游标示例，在终端窗口中，转到*INSTALL_DIR*`/jaxp-`*version*`/samples/`目录，并输入以下内容：**

    ```java
    javac stax/cursor/*.java

    ```

1.  **在`BookCatalogue.xml`文件上运行`CursorParse`示例，使用以下命令。**

    `CursorParse`将打印出`BookCatalogue.xml`文件的每个元素。

```java
java stax/event/CursorParse stax/data/BookCatalogue.xml

```

## 游标到事件示例

位于*tut-install*`/javaeetutorial5/examples/stax/cursor2event/`目录中，`CursorApproachEventObject.java`演示了如何在使用游标 API 时获取`XMLEvent`对象返回的信息。

这里的想法是，游标 API 的`XMLStreamReader`返回与特定事件对应的整数常量，而事件迭代器 API 的`XMLEventReader`返回不可变且持久的事件对象。 `XMLStreamReader`更有效率，但`XMLEventReader`更易于使用，因为与特定事件相关的所有信息都封装在返回的`XMLEvent`对象中。然而，事件方法的缺点是为每个事件创建对象的额外开销，这既消耗时间又消耗内存。

有了这个想法，即使使用游标 API，也可以使用`XMLEventAllocator`来获取事件信息作为`XMLEvent`对象。

### 实例化一个 XMLEventAllocator

第一步是创建一个新的`XMLInputFactory`并实例化一个`XMLEventAllocator`：

```java
XMLInputFactory xmlif = XMLInputFactory.newInstance();
System.out.println("FACTORY: " + xmlif);
xmlif.setEventAllocator(new XMLEventAllocatorImpl());
allocator = xmlif.getEventAllocator();
XMLStreamReader xmlr = xmlif.createXMLStreamReader(filename,
                           new FileInputStream(filename));

```

### 创建一个事件迭代器

下一步是创建一个事件迭代器：

```java
int eventType = xmlr.getEventType();

while (xmlr.hasNext()) {
    eventType = xmlr.next();
    // Get all "Book" elements as XMLEvent object
    if (eventType == XMLStreamConstants.START_ELEMENT 
        && xmlr.getLocalName().equals("Book")) {
        // get immutable XMLEvent
        StartElement event = getXMLEvent(xmlr).asStartElement();
        System.out.println ("EVENT: " + event.toString());
    }
}

```

### 创建分配器方法

最后一步是创建`XMLEventAllocator`方法：

```java
private static XMLEvent getXMLEvent(XMLStreamReader reader)
    throws XMLStreamException {
    return allocator.allocate(reader);
}

```

### 运行游标到事件示例

1.  **要编译和运行游标到事件示例，在终端窗口中，转到*INSTALL_DIR*`/jaxp-`*version*`/samples/`目录，并输入以下内容：**

    ```java
    javac -classpath ../lib/jaxp-ri.jar stax/cursor2event/*.java

    ```

1.  **在`BookCatalogue.xml`文件上运行`CursorApproachEventObject`示例，使用以下命令。**

    ```java
    java stax/cursor2event/CursorApproachEventObject stax/data/BookCatalogue.xml

    ```

    `CursorApproachEventObject`将打印出`BookCatalogue.xml`文件中定义的事件列表。

## 事件示例

位于*INSTALL_DIR*`/jaxp-`*version*`/samples/stax/event/`目录中，`EventParse.java`演示了如何使用 StAX 事件 API 读取 XML 文档。

### 创建一个输入工厂

第一步是创建一个新的`XMLInputFactory`实例：

```java
XMLInputFactory factory = XMLInputFactory.newInstance();
System.out.println("FACTORY: " + factory);

```

### 创建一个事件读取器

下一步是创建一个 `XMLEventReader` 实例：

```java
XMLEventReader r = factory.createXMLEventReader
                       (filename, new FileInputStream(filename));

```

### 创建一个事件迭代器

第三步是创建一个事件迭代器：

```java
XMLEventReader r = factory.createXMLEventReader
                       (filename, new FileInputStream(filename));
while (r.hasNext()) {
    XMLEvent e = r.nextEvent();
    System.out.println(e.toString());
}

```

### 获取事件流

最后一步是获取底层事件流：

```java
public final static String getEventTypeString(int eventType) {
    switch (eventType) {
        case XMLEvent.START_ELEMENT:
            return "START_ELEMENT";

        case XMLEvent.END_ELEMENT:
            return "END_ELEMENT";

        case XMLEvent.PROCESSING_INSTRUCTION:
            return "PROCESSING_INSTRUCTION";

        case XMLEvent.CHARACTERS:
            return "CHARACTERS";

        case XMLEvent.COMMENT:
            return "COMMENT";

        case XMLEvent.START_DOCUMENT:
            return "START_DOCUMENT";

        case XMLEvent.END_DOCUMENT:
            return "END_DOCUMENT";

        case XMLEvent.ENTITY_REFERENCE:
            return "ENTITY_REFERENCE";

        case XMLEvent.ATTRIBUTE:
            return "ATTRIBUTE";

        case XMLEvent.DTD:
            return "DTD";

        case XMLEvent.CDATA:
            return "CDATA";

        case XMLEvent.SPACE:
            return "SPACE";
    }
    return "UNKNOWN_EVENT_TYPE," + eventType;
}

```

### 返回输出

当你运行事件示例时，`EventParse` 类被编译，XML 流被解析为事件并返回到 `STDOUT`。例如，`Author` 元素的一个实例被返回为：

```java
<[’http://www.publishing.org’]::Author>
    Dhirendra Brahmachari
</[’http://www.publishing.org’]::Author>

```

请注意，在这个示例中，事件包括一个包含命名空间的开标签和闭标签，两者都包含元素的内容作为字符串返回在标签内。

同样，一个 `Cost` 元素的一个实例被返回如下：

```java
<[’http://www.publishing.org’]::Cost currency=’INR’>
    11.50
</[’http://www.publishing.org’]::Cost

```

在这种情况下，`currency` 属性和值在事件的开标签中返回。

### 运行事件示例

1.  **要编译和运行事件示例，在终端窗口中，转到 *INSTALL_DIR*`/jaxp-`*version*`/samples/` 目录并输入以下内容：**

    ```java
    javac -classpath ../lib/jaxp-ri.jar stax/event/*.java

    ```

1.  **对 `BookCatalogue.xml` 文件运行 `EventParse` 示例，使用以下命令。**

    ```java
    java stax/event/EventParse stax/data/BookCatalogue.xml

    ```

    `EventParse` 将打印出由 `BookCatalogue.xml` 文件定义的所有元素的数据。

## 过滤器示例

位于 *INSTALL_DIR*`/jaxp-`*version*`/samples/stax/filter/` 目录中，`MyStreamFilter.java` 演示了如何使用 StAX 流过滤器 API 过滤应用程序不需要的事件。在这个示例中，解析器过滤掉除了 `StartElement` 和 `EndElement` 之外的所有事件。

### 实现 StreamFilter 类

`MyStreamFilter` 类实现了 `javax.xml.stream.StreamFilter`：

```java
public class MyStreamFilter implements javax.xml.stream.StreamFilter {
    // ...
}

```

### 创建一个输入工厂

下一步是创建一个 `XMLInputFactory` 实例。在这种情况下，还在工厂上设置了各种属性：

```java
XMLInputFactory xmlif = null ;

try {
    xmlif = XMLInputFactory.newInstance();
    xmlif.setProperty(
        XMLInputFactory.IS_REPLACING_ENTITY_REFERENCES,
        Boolean.TRUE);

    xmlif.setProperty(
        XMLInputFactory.IS_SUPPORTING_EXTERNAL_ENTITIES,
        Boolean.FALSE);

    xmlif.setProperty(
        XMLInputFactory.IS_NAMESPACE_AWARE,
        Boolean.TRUE);

    xmlif.setProperty(
        XMLInputFactory.IS_COALESCING,
        Boolean.TRUE);
} 
catch (Exception ex) {
    ex.printStackTrace();
}

System.out.println("FACTORY: " + xmlif);
System.out.println("filename = "+ filename);

```

### 创建过滤器

下一步是实例化一个文件输入流并创建流过滤器：

```java
FileInputStream fis = new FileInputStream(filename);
XMLStreamReader xmlr = xmlif.createFilteredReader(
                           xmlif.createXMLStreamReader(fis), 
                           new MyStreamFilter());

int eventType = xmlr.getEventType();
printEventType(eventType);

while (xmlr.hasNext()) {
    eventType = xmlr.next();
    printEventType(eventType);
    printName(xmlr,eventType);
    printText(xmlr);

    if (xmlr.isStartElement()) {
        printAttributes(xmlr);
    }
    printPIData(xmlr);
    System.out.println("-----------------------");
}

```

### 捕获事件流

下一步是捕获事件流。这与事件示例中的方式基本相同。

### 过滤流

最后一步是过滤流：

```java
public boolean accept(XMLStreamReader reader) {
    if (!reader.isStartElement() && !reader.isEndElement())
        return false;
    else
        return true;
}

```

### 返回输出

当你运行过滤器示例时，`MyStreamFilter` 类被编译，XML 流被解析为事件并返回到 `STDOUT`。例如，一个 `Author` 事件被返回如下：

```java
EVENT TYPE(1):START_ELEMENT
HAS NAME: Author
HAS NO TEXT
HAS NO ATTRIBUTES
-----------------------------
EVENT TYPE(2):END_ELEMENT
HAS NAME: Author
HAS NO TEXT
-----------------------------

```

同样，一个 `Cost` 事件被返回如下：

```java
EVENT TYPE(1):START_ELEMENT
HAS NAME: Cost
HAS NO TEXT

HAS ATTRIBUTES:
 ATTRIBUTE-PREFIX:
 ATTRIBUTE-NAMESP: null
ATTRIBUTE-NAME:   currency
ATTRIBUTE-VALUE: USD
ATTRIBUTE-TYPE:  CDATA

-----------------------------
EVENT TYPE(2):END_ELEMENT
HAS NAME: Cost
HAS NO TEXT
-----------------------------

```

查看 迭代器 API 和 读取 XML 流 以获取更详细的 StAX 事件解析讨论。

### 运行过滤器示例

1.  **要编译和运行过滤器示例，在终端窗口中，转到 *INSTALL_DIR*`/jaxp-`*version*`/samples/` 目录并输入以下内容：**

    ```java
    javac -classpath ../lib/jaxp-ri.jar stax/filter/*.java

    ```

1.  **在 `java.endorsed.dirs` 系统属性设置为指向 `samples/lib` 目录的情况下，对 `BookCatalogue.xml` 文件运行 `MyStreamFilter` 示例，使用以下命令。**

    ```java
    java -Djava.endorsed.dirs=../lib stax/filter/MyStreamFilter -f stax/data/BookCatalogue.xml

    ```

    `MyStreamFilter` 将打印出由 `BookCatalogue.xml` 文件定义的事件作为 XML 流。

## 读写示例

位于*INSTALL_DIR*`/jaxp-`*version*`/samples/stax/readnwrite/`目录中，`EventProducerConsumer.java`演示了如何同时将 StAX 解析器用作生产者和消费者。

StAX `XMLEventWriter` API 扩展自`XMLEventConsumer`接口，并被称为**事件消费者**。相比之下，`XMLEventReader`是一个**事件生产者**。StAX 支持同时读取和写入，因此可以顺序地从一个 XML 流中读取并同时写入到另一个流中。

读写示例展示了如何使用 StAX 生产者/消费者机制同时读取和写入。该示例还展示了如何修改流以及如何动态添加新事件，然后写入到不同的流中。

### 创建一个事件生产者/消费者

第一步是实例化一个事件工厂，然后创建一个事件生产者/消费者的实例：

```java
XMLEventFactory m_eventFactory = XMLEventFactory.newInstance();

public EventProducerConsumer() {
    // ...
    try {
        EventProducerConsumer ms = new EventProducerConsumer();

        XMLEventReader reader = XMLInputFactory.newInstance().
        createXMLEventReader(new java.io.FileInputStream(args[0]));

        XMLEventWriter writer = 
            XMLOutputFactory.newInstance().createXMLEventWriter(System.out);
    }
    // ...
}

```

### 创建一个迭代器

下一步是创建一个迭代器来解析流：

```java
while (reader.hasNext()) {
    XMLEvent event = (XMLEvent)reader.next();
    if (event.getEventType() == event.CHARACTERS) {
        writer.add(ms.getNewCharactersEvent(event.asCharacters()));
    }
    else {
        writer.add(event);
    }
}
writer.flush();

```

### 创建一个写入器

最后一步是创建一个流写入器，形式为一个新的`Character`事件：

```java
Characters getNewCharactersEvent(Characters event) {
    if (event.getData().equalsIgnoreCase("Name1")) {
        return m_eventFactory.createCharacters(
                   Calendar.getInstance().getTime().toString());
    }
    // else return the same event
    else {
        return event;
    }
}

```

### 返回输出

运行读写示例时，`EventProducerConsumer`类被编译，并且 XML 流被解析为事件并写回到`STDOUT`。输出是示例 XML 文档中描述的`BookCatalog.xml`文件的内容。

### 运行读写示例

1.  **要编译和运行读写示例，在终端窗口中，转到*INSTALL_DIR*`/jaxp-`*version*`/samples/`目录并输入以下内容：**

    ```java
    javac -classpath ../lib/jaxp-ri.jar stax/readnwrite/*.java

    ```

1.  **在`BookCatalogue.xml`文件上运行`EventProducerConsumer`示例，使用以下命令。**

    ```java
    java stax/readnwrite/EventProducerConsumer stax/data/BookCatalogue.xml

    ```

    `EventProducerConsumer`将打印出`BookCatalogue.xml`文件的内容。

## 写入器示例

位于*INSTALL_DIR*`/jaxp-`*version*`/samples/stax/writer/`目录中，`CursorWriter.java`演示了如何使用 StAX 游标 API 编写 XML 流。

### 创建输出工厂

第一步是创建一个`XMLOutputFactory`的实例：

```java
XMLOutputFactory xof =  XMLOutputFactory.newInstance();

```

### 创建一个流写入器

下一步是创建一个`XMLStreamWriter`的实例：

```java
XMLStreamWriter xtw = null;

```

### 写入流

最后一步是写入 XML 流。请注意，在写入最终的`EndDocument`后，流会被刷新并关闭：

```java
xtw = xof.createXMLStreamWriter(new FileWriter(fileName));

xtw.writeComment("all elements here are explicitly in the HTML namespace");
xtw.writeStartDocument("utf-8","1.0");
xtw.setPrefix("html", "http://www.w3.org/TR/REC-html40");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40","html");
xtw.writeNamespace("html", "http://www.w3.org/TR/REC-html40");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "head");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "title");
xtw.writeCharacters("Frobnostication");
xtw.writeEndElement();
xtw.writeEndElement();

xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "body");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "p");
xtw.writeCharacters("Moved to");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "a");
xtw.writeAttribute("href","http://frob.com");
xtw.writeCharacters("here");
xtw.writeEndElement();
xtw.writeEndElement();
xtw.writeEndElement();
xtw.writeEndElement();
xtw.writeEndDocument();

xtw.flush();
xtw.close();

```

### 返回输出

运行写入器示例时，`CursorWriter`类被编译，并且 XML 流被解析为事件并写入到名为`dist/CursorWriter-Output`的文件中：

```java
<!--all elements here are explicitly in the HTML namespace-->
<?xml version="1.0" encoding="utf-8"?>
<html:html >
<html:head>
<html:title>Frobnostication</html:title></html:head>
<html:body>
<html:p>Moved to <html:a href="http://frob.com">here</html:a>
</html:p>
</html:body>
</html:html>

```

在实际的`dist/CursorWriter-Output`文件中，该流是连续写入的，没有任何换行符；这里添加了换行符以便更容易阅读清单。在这个示例中，与事件示例中的对象流一样，命名空间前缀被添加到 HTML 标签的开头和结尾。虽然 StAX 规范不要求添加这个前缀，但是当输出流的最终范围不明确时，这是一个良好的实践。

### 运行写入器示例

1.  **要编译和运行 Writer 示例，在终端窗口中，转到 *INSTALL_DIR*`/jaxp-`*version*`/samples/` 目录，并输入以下内容：**

    ```java
    javac -classpath \
        ../lib/jaxp-ri.jar stax/writer/*.java

    ```

1.  **运行 `CursorWriter` 示例，指定输出应写入的文件名。**

    ```java
    java stax/writer/CursorWriter -f *output_file*

    ```

    `CursorWriter` 将创建一个包含 返回输出 中显示的数据的相应名称的输出文件。
