# 如何用 Java 编写 XML 文件(StAX Writer)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-write-xml-file-in-java-stax-writer/>

本教程展示了如何使用[Streaming API for XML(StAX)](http://web.archive.org/web/20220723135335/https://docs.oracle.com/javase/tutorial/jaxp/stax/index.html)writer 将数据写入 XML 文件。

目录

*   [1。写入 XML (StAX 编写器 API)](#write-to-xml-stax-writer-apis)
*   [2。写入 XML(StAX Cursor API–XML streamwriter)](#write-to-xml-stax-cursor-api-xmlstreamwriter)
*   [3。写入 XML (StAX 迭代器 API–XMLEventWriter)](#write-to-xml-stax-iterator-api-xmleventwriter)
*   [4。编写并漂亮打印 XML 内容(Transformer)](#write-and-pretty-print-xml-content-transformer)
*   [5。下载源代码](#download-source-code)
*   [6。参考文献](#references)

**注意**
[如何在 Java 中读取 XML 文件(StAX Parser)](http://web.archive.org/web/20220723135335/https://mkyong.com/java/how-to-read-xml-file-in-java-stax-parser/)

下面所有的例子都是用 Java 11 测试的。

## 1。写入 XML (StAX 编写器 API)

在 Streaming API for XML (StAX)中，我们可以使用`StAX Cursor API`或`StAX Iterator API`将数据写入 XML 文件。

1.1 下面是将数据写入 XML 的`StAX Cursor API`。

```java
 XMLOutputFactory output = XMLOutputFactory.newInstance();

  XMLStreamWriter writer = output.createXMLStreamWriter(
                              new FileOutputStream("/path/test.xml"));

  writer.writeStartDocument();

  writer.writeStartElement("test");

  // write other XML elements

  writer.writeEndDocument();

  writer.flush();

  writer.close(); 
```

1.2 下面是将数据写入 XML 的`StAX Iterator API`。

```java
 XMLOutputFactory output = XMLOutputFactory.newInstance();

  XMLEventFactory eventFactory = XMLEventFactory.newInstance();

  XMLEventWriter writer = output.createXMLEventWriter(
                            new FileOutputStream("/path/test.xml"));

  XMLEvent event = eventFactory.createStartDocument();
  writer.add(event);

  // add other xml events

  writer.add(eventFactory.createEndDocument());

  writer.flush();

  writer.close(); 
```

## 2。写入 XML(StAX Cursor API–XML streamwriter)

以下示例使用 StAX Cursor API `XMLStreamWriter`将数据写入 XML 文件。

WriteXmlStAXCursor.java

```java
 package com.mkyong.xml.stax;

import javax.xml.stream.XMLOutputFactory;
import javax.xml.stream.XMLStreamException;
import javax.xml.stream.XMLStreamWriter;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;

public class WriteXmlStAXCursor {

    public static void main(String[] args) throws XMLStreamException {

        // send the output to a xml file
        try(FileOutputStream out = new FileOutputStream("/home/mkyong/test.xml")){
            writeXml(out);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // send the output to System.out
        // writeXml(System.out);

    }

    // StAX Cursor API
    private static void writeXml(OutputStream out) throws XMLStreamException {

        XMLOutputFactory output = XMLOutputFactory.newInstance();

        XMLStreamWriter writer = output.createXMLStreamWriter(out);

        writer.writeStartDocument("utf-8", "1.0");

        // <company>
        writer.writeStartElement("company");

        // <staff>

        // add XML comment
        writer.writeComment("This is Staff 1001");

        writer.writeStartElement("staff");
        writer.writeAttribute("id", "1001");

        writer.writeStartElement("name");
        writer.writeCharacters("mkyong");
        writer.writeEndElement();

        writer.writeStartElement("salary");
        writer.writeAttribute("currency", "USD");
        writer.writeCharacters("5000");
        writer.writeEndElement();

        writer.writeStartElement("bio");
        writer.writeCData("HTML tag <code>testing</code>");
        writer.writeEndElement();

        writer.writeEndElement();
        // </staff>

        // <staff>
        writer.writeStartElement("staff");
        writer.writeAttribute("id", "1002");

        writer.writeStartElement("name");
        writer.writeCharacters("yflow");
        writer.writeEndElement();

        writer.writeStartElement("salary");
        writer.writeAttribute("currency", "EUR");
        writer.writeCharacters("8000");
        writer.writeEndElement();

        writer.writeStartElement("bio");
        writer.writeCData("a & b");
        writer.writeEndElement();

        writer.writeEndElement();
        // </staff>

        writer.writeEndDocument();
        // </company>

        writer.flush();

        writer.close();

    }

} 
```

`XMLStreamWriter`以压缩模式将数据写入 XML 文件，稍后我们将使用`Transformer`来[打印 XML](#write-and-pretty-print-xml-content-transformer) 内容。

/home/mkyong/test.xml

```java
 <?xml version="1.0" encoding="utf-8"?><company>
<!--This is Staff 1001--><staff id="1001"><name>mkyong</name><salary currency="USD">5000</salary>
<bio><![CDATA[HTML tag <code>testing</code>]]></bio></staff>
<staff id="1002"><name>yflow</name><salary currency="EUR">8000</salary>
<bio><![CDATA[a & b]]></bio></staff></company> 
```

## 3。写入 XML (StAX 迭代器 API–XMLEventWriter)

下面的例子使用 StAX 迭代器 API `XMLEventWriter`将数据写入 XML 文件。

WriteXmlStAXIterator.java

```java
 package com.mkyong.xml.stax;

import javax.xml.stream.*;
import javax.xml.stream.events.XMLEvent;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;

public class WriteXmlStAXIterator {

    public static void main(String[] args) throws XMLStreamException {

        // send the output to a xml file
        try(FileOutputStream out = new FileOutputStream("/home/mkyong/test.xml")){
            writeXml2(out);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // send the output to System.out
        // writeXml2(System.out);

    }

    // StAX Iterator API
    private static void writeXml2(OutputStream out) throws XMLStreamException {

        XMLOutputFactory output = XMLOutputFactory.newInstance();
        XMLEventFactory eventFactory = XMLEventFactory.newInstance();

        // StAX Iterator API
        XMLEventWriter writer = output.createXMLEventWriter(out);

        XMLEvent event = eventFactory.createStartDocument();
        // default
        //event = eventFactory.createStartDocument("utf-8", "1.0");
        writer.add(event);

        writer.add(eventFactory.createStartElement("", "", "company"));

        // write XML comment
        writer.add(eventFactory.createComment("This is staff 1001"));

        writer.add(eventFactory.createStartElement("", "", "staff"));
        // write XML attribute
        writer.add(eventFactory.createAttribute("id", "1001"));

        writer.add(eventFactory.createStartElement("", "", "name"));
        writer.add(eventFactory.createCharacters("mkyong"));
        writer.add(eventFactory.createEndElement("", "", "name"));

        writer.add(eventFactory.createStartElement("", "", "salary"));
        writer.add(eventFactory.createAttribute("currency", "USD"));
        writer.add(eventFactory.createCharacters("5000"));
        writer.add(eventFactory.createEndElement("", "", "salary"));

        writer.add(eventFactory.createStartElement("", "", "bio"));
        // write XML CData
        writer.add(eventFactory.createCData("HTML tag <code>testing</code>"));
        writer.add(eventFactory.createEndElement("", "", "bio"));

        // </staff>
        writer.add(eventFactory.createEndElement("", "", "staff"));

        // next staff, tired to write more
        // writer.add(eventFactory.createStartElement("", "", "staff"));
        // writer.add(eventFactory.createAttribute("id", "1002"));
        // writer.add(eventFactory.createEndElement("", "", "staff"));

        // end here.
        writer.add(eventFactory.createEndDocument());

        writer.flush();

        writer.close();

    }

} 
```

输出

/home/mkyong/test.xml

```java
 <?xml version="1.0" encoding="UTF-8"?><company>
<!--This is staff 1001--><staff id="1001"><name>mkyong</name>
<salary currency="USD">5000</salary>
<bio><![CDATA[HTML tag <code>testing</code>]]></bio>
</staff></company> 
```

## 4。编写并漂亮打印 XML 内容(Transformer)

4.1 下面的代码片段使用`javax.xml.transform.Transformer`来打印一个 XML 内容。

```java
 private static String formatXML(String xml) throws TransformerException {

      // write data to xml file
      TransformerFactory transformerFactory = TransformerFactory.newInstance();

      Transformer transformer = transformerFactory.newTransformer();

      // alternative
      //Transformer transformer = SAXTransformerFactory.newInstance().newTransformer();

      // pretty print by indention
      transformer.setOutputProperty(OutputKeys.INDENT, "yes");

      // add standalone="yes", add line break before the root element
      transformer.setOutputProperty(OutputKeys.STANDALONE, "yes");

      /*transformer.setOutputProperty(OutputKeys.DOCTYPE_PUBLIC,
              "-//W3C//DTD XHTML 1.0 Transitional//EN");

      transformer.setOutputProperty(OutputKeys.DOCTYPE_SYSTEM,
              "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd");*/

      // ignore <?xml version="1.0" encoding="UTF-8"?>
      //transformer.setOutputProperty(OutputKeys.OMIT_XML_DECLARATION, "yes");

      StreamSource source = new StreamSource(new StringReader(xml));
      StringWriter output = new StringWriter();
      transformer.transform(source, new StreamResult(output));

      return output.toString();

  } 
```

4.2 美化 XML 内容的步骤。

1.  将数据写入`ByteArrayOutputStream`变量。
2.  将`ByteArrayOutputStream`变量转换成字符串。
3.  `Transformer`输出字符串并返回一个格式化的 XML 字符串。
4.  将格式化的 XML 字符串保存到文件中。

下面的例子打印 XML 内容并写入一个 XML 文件。

WriteXmlStAXPrettyPrint.java

```java
 package com.mkyong.xml.stax;

import javax.xml.stream.XMLOutputFactory;
import javax.xml.stream.XMLStreamException;
import javax.xml.stream.XMLStreamWriter;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.stream.StreamResult;
import javax.xml.transform.stream.StreamSource;
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Paths;

public class WriteXmlStAXPrettyPrint {

    public static void main(String[] args) {

        try {

            ByteArrayOutputStream out = new ByteArrayOutputStream();
            // write XML to ByteArrayOutputStream
            writeXml(out);

            // Java 10
            //String xml = out.toString(StandardCharsets.UTF_8);

            // standard way to convert byte array to String
            String xml = new String(out.toByteArray(), StandardCharsets.UTF_8);

            // System.out.println(formatXML(xml));

            String prettyPrintXML = formatXML(xml);

            // print to console
            // System.out.println(prettyPrintXML);

            // Java 11 - write to file
            Files.writeString(Paths.get("/home/mkyong/test.xml"),
                    prettyPrintXML, StandardCharsets.UTF_8);

            // Java 7 - write to file
            //Files.write(Paths.get("/home/mkyong/test.xml"),
            //    prettyPrintXML.getBytes(StandardCharsets.UTF_8));

            // BufferedWriter - write to file
            /*try (FileWriter writer = new FileWriter("/home/mkyong/test.xml");
                 BufferedWriter bw = new BufferedWriter(writer)) {
                bw.write(xml);
            } catch (IOException e) {
                e.printStackTrace();
            }*/

        } catch (TransformerException | XMLStreamException | IOException e) {
            e.printStackTrace();
        }

    }

    // StAX Cursor API
    private static void writeXml(OutputStream out) throws XMLStreamException {

        XMLOutputFactory output = XMLOutputFactory.newInstance();

        XMLStreamWriter writer = output.createXMLStreamWriter(out);

        writer.writeStartDocument("utf-8", "1.0");

        // <company>
        writer.writeStartElement("company");

        // <staff>
        // add XML comment
        writer.writeComment("This is Staff 1001");

        writer.writeStartElement("staff");
        writer.writeAttribute("id", "1001");

        writer.writeStartElement("name");
        writer.writeCharacters("mkyong");
        writer.writeEndElement();

        writer.writeStartElement("salary");
        writer.writeAttribute("currency", "USD");
        writer.writeCharacters("5000");
        writer.writeEndElement();

        writer.writeStartElement("bio");
        writer.writeCData("HTML tag <code>testing</code>");
        writer.writeEndElement();

        writer.writeEndElement();
        // </staff>

        // <staff>
        writer.writeStartElement("staff");
        writer.writeAttribute("id", "1002");

        writer.writeStartElement("name");
        writer.writeCharacters("yflow");
        writer.writeEndElement();

        writer.writeStartElement("salary");
        writer.writeAttribute("currency", "EUR");
        writer.writeCharacters("8000");
        writer.writeEndElement();

        writer.writeStartElement("bio");
        writer.writeCData("a & b");
        writer.writeEndElement();

        writer.writeEndElement();
        // </staff>

        writer.writeEndDocument();
        // </company>

        writer.flush();

        writer.close();

    }

    private static String formatXML(String xml) throws TransformerException {

        // write data to xml file
        TransformerFactory transformerFactory = TransformerFactory.newInstance();
        Transformer transformer = transformerFactory.newTransformer();

        // pretty print by indention
        transformer.setOutputProperty(OutputKeys.INDENT, "yes");

        // add standalone="yes", add line break before the root element
        transformer.setOutputProperty(OutputKeys.STANDALONE, "yes");

        StreamSource source = new StreamSource(new StringReader(xml));
        StringWriter output = new StringWriter();
        transformer.transform(source, new StreamResult(output));

        return output.toString();

    }

} 
```

输出

/home/mkyong/test.xml

```java
 <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<company>
    <!--This is Staff 1001-->
    <staff id="1001">
        <name>mkyong</name>
        <salary currency="USD">5000</salary>
        <bio>
            <![CDATA[HTML tag <code>testing</code>]]>
        </bio>
    </staff>
    <staff id="1002">
        <name>yflow</name>
        <salary currency="EUR">8000</salary>
        <bio>
            <![CDATA[a & b]]>
        </bio>
    </staff>
</company> 
```

**将 XML 转换成 Java 对象**
试试 [Jakarta XML Binding (JAXB)](http://web.archive.org/web/20220723135335/https://mkyong.com/java/jaxb-hello-world-example/) 将 XML 转换成 Java 对象/从 Java 对象转换过来。

## 5。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220723135335/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/StAX/

## 6。参考文献

*   [维基百科–用于 XML 处理的 Java API](http://web.archive.org/web/20220723135335/https://en.wikipedia.org/wiki/Java_API_for_XML_Processing)
*   [XML 流应用编程接口(StAX)](http://web.archive.org/web/20220723135335/https://docs.oracle.com/javase/tutorial/jaxp/stax/index.html)
*   [StAX 简介](http://web.archive.org/web/20220723135335/https://www.xml.com/pub/a/2003/09/17/stax.html)
*   [如何在 Java 中读取 XML 文件(StAX 解析器)](http://web.archive.org/web/20220723135335/https://mkyong.com/java/how-to-read-xml-file-in-java-stax-parser/)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220723135335/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何在 Java 中读取 XML 文件—(SAX 解析器)](http://web.archive.org/web/20220723135335/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [如何在 Java 中读取 XML 文件—(JDOM 解析器)](http://web.archive.org/web/20220723135335/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220723135335/https://mkyong.com/java/jaxb-hello-world-example/)
*   [Java–如何将字符串保存到文件中](http://web.archive.org/web/20220723135335/https://mkyong.com/java/java-how-to-save-a-string-to-a-file/)
*   [Apache Xerces 常见问题解答](http://web.archive.org/web/20220723135335/https://xerces.apache.org/xerces2-j/faq-general.html#faq-6)

<input type="hidden" id="mkyong-current-postId" value="16745">