# Java–将字符串转换成 XML

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-convert-string-to-xml/>

本文展示了如何使用 DOM parser 和 JDOM2 parser 在 Java 中将字符串转换成 XML 文档，并将 XML 文档转换回字符串。

*   [1。将字符串转换为 XML (DOM 解析器)](#convert-string-to-xml-dom-parser)
*   [2。将字符串转换为 XML (JDOM2 解析器)](#convert-string-to-xml-jdom2-parser)
*   [3。下载源代码](#download-source-code)
*   [4。参考文献](#references)

## 1。将字符串转换为 XML (DOM 解析器)

这个例子展示了如何使用 DOM 解析器将字符串转换成 XML 文档，然后再转换成字符串。

ConvertStringToXmlDom.java

```java
 package com.mkyong.xml.tips;

import org.w3c.dom.Document;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;

import javax.xml.XMLConstants;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.IOException;
import java.io.StringReader;
import java.io.StringWriter;

// DOM Parser
public class ConvertStringToXmlDom {

    final static String str = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\n" +
            "<company>\n" +
            "    <staff id=\"1001\">\n" +
            "        <name>mkyong</name>\n" +
            "        <role>support</role>\n" +
            "        <salary currency=\"USD\">5000</salary>\n" +
            "        <!-- for special characters like < &, need CDATA -->\n" +
            "        <bio><![CDATA[HTML tag <code>testing</code>]]></bio>\n" +
            "    </staff>\n" +
            "    <staff id=\"1002\">\n" +
            "        <name>yflow</name>\n" +
            "        <role>admin</role>\n" +
            "        <salary currency=\"EUR\">8000</salary>\n" +
            "        <bio><![CDATA[a & b]]></bio>\n" +
            "    </staff>\n" +
            "</company>";

    public static void main(String[] args) {

        // String to XML Document
        Document document = convertStringToXml(str);

        // XML Document to String
        String xml = convertXmlToString(document);

        System.out.println(xml);

    }

    private static String convertXmlToString(Document doc) {
        DOMSource domSource = new DOMSource(doc);
        StringWriter writer = new StringWriter();
        StreamResult result = new StreamResult(writer);
        TransformerFactory tf = TransformerFactory.newInstance();
        Transformer transformer = null;
        try {
            transformer = tf.newTransformer();
            transformer.transform(domSource, result);
        } catch (TransformerException e) {
            throw new RuntimeException(e);
        }
        return writer.toString();
    }

    private static Document convertStringToXml(String xmlString) {

        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

        try {

            // optional, but recommended
            // process XML securely, avoid attacks like XML External Entities (XXE)
            dbf.setFeature(XMLConstants.FEATURE_SECURE_PROCESSING, true);

            DocumentBuilder builder = dbf.newDocumentBuilder();

            Document doc = builder.parse(new InputSource(new StringReader(xmlString)));

            return doc;

        } catch (ParserConfigurationException | IOException | SAXException e) {
            throw new RuntimeException(e);
        }

    }

} 
```

输出

Terminal

```java
 <?xml version="1.0" encoding="utf-8" standalone="no"?><company>
    <staff id="1001">
        <name>mkyong</name>
        <role>support</role>
        <salary currency="USD">5000</salary>
        <!-- for special characters like < &, need CDATA -->
        <bio><![CDATA[HTML tag <code>testing</code>]]></bio>
    </staff>
    <staff id="1002">
        <name>yflow</name>
        <role>admin</role>
        <salary currency="EUR">8000</salary>
        <bio><![CDATA[a & b]]></bio>
    </staff>
</company> 
```

## 2。将字符串转换为 XML (JDOM2 解析器)

这个例子展示了如何使用 JDOM2 解析器将字符串转换成 XML 文档，然后再转换回字符串。

pom.xml

```java
 <dependency>
    <groupId>org.jdom</groupId>
    <artifactId>jdom2</artifactId>
    <version>2.0.6</version>
  </dependency> 
```

ConvertStringToXmlJDom2.java

```java
 package com.mkyong.xml.tips;

import org.jdom2.Document;
import org.jdom2.JDOMException;
import org.jdom2.input.SAXBuilder;
import org.jdom2.output.Format;
import org.jdom2.output.XMLOutputter;

import javax.xml.XMLConstants;
import java.io.IOException;
import java.io.StringReader;
import java.io.StringWriter;

// JDOM2 Parser
public class ConvertStringToXmlJDom2 {

    final static String str = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\n" +
            "<company>\n" +
            "    <staff id=\"1001\">\n" +
            "        <name>mkyong</name>\n" +
            "        <role>support</role>\n" +
            "        <salary currency=\"USD\">5000</salary>\n" +
            "        <!-- for special characters like < &, need CDATA -->\n" +
            "        <bio><![CDATA[HTML tag <code>testing</code>]]></bio>\n" +
            "    </staff>\n" +
            "    <staff id=\"1002\">\n" +
            "        <name>yflow</name>\n" +
            "        <role>admin</role>\n" +
            "        <salary currency=\"EUR\">8000</salary>\n" +
            "        <bio><![CDATA[a & b]]></bio>\n" +
            "    </staff>\n" +
            "</company>";

    public static void main(String[] args) {

        // String to XML Document
        Document document = convertStringToXml(str);

        // XML Document to String
        String xml = convertXmlDocumentToString(document);

        System.out.println(xml);

    }

    private static String convertXmlDocumentToString(Document doc) {
        XMLOutputter xmlOutput = new XMLOutputter();
        xmlOutput.setFormat(Format.getPrettyFormat());

        StringWriter result = new StringWriter();
        try {
            xmlOutput.output(doc, result);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        return result.toString();
    }

    private static Document convertStringToXml(String xmlString) {

        SAXBuilder sax = new SAXBuilder();

        // https://rules.sonarsource.com/java/RSPEC-2755
        // prevent xxe
        sax.setProperty(XMLConstants.ACCESS_EXTERNAL_DTD, "");
        sax.setProperty(XMLConstants.ACCESS_EXTERNAL_SCHEMA, "");

        try {

            Document doc = sax.build(new StringReader(xmlString));
            return doc;

        } catch (JDOMException | IOException e) {
            throw new RuntimeException(e);
        }

    }

} 
```

输出

Terminal

```java
 <?xml version="1.0" encoding="UTF-8"?>
  <company>
    <staff id="1001">
      <name>mkyong</name>
      <role>support</role>
      <salary currency="USD">5000</salary>
      <!-- for special characters like < &, need CDATA -->
      <bio><![CDATA[HTML tag <code>testing</code>]]></bio>
    </staff>
    <staff id="1002">
      <name>yflow</name>
      <role>admin</role>
      <salary currency="EUR">8000</salary>
      <bio><![CDATA[a & b]]></bio>
    </staff>
  </company> 
```

## 3。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221230032525/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/tips/

## 4。参考文献

*   [Oracle–文档对象模型](http://web.archive.org/web/20221230032525/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [如何在 Java 中读取 XML 文件—(SAX 解析器)](http://web.archive.org/web/20221230032525/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [如何在 Java 中读取 XML 文件—(JDOM 解析器)](http://web.archive.org/web/20221230032525/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
*   [JAXB hello world 示例](http://web.archive.org/web/20221230032525/https://mkyong.com/java/jaxb-hello-world-example/)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20221230032525/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)

<input type="hidden" id="mkyong-current-postId" value="17162">