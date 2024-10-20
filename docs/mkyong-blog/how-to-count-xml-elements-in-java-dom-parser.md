# 如何在 Java 中计算 XML 元素

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-count-xml-elements-in-java-dom-parser/>

本文展示了如何使用 [DOM](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Document_Object_Model) 解析器、 [SAX](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Simple_API_for_XML) 解析器和 [XPath](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/XPath) 来计算 Java 中指定 XML 元素的数量。

目录

*   [1。一个 XML 文件](#an-xml-file)
*   [2。统计 XML 元素(DOM 解析器)](#count-the-xml-elements-dom-parser)
*   [3。计数 XML 元素(XPath)](#count-the-xml-elements-xpath)
*   [4。统计 XML 元素(SAX 解析器)](#count-the-xml-elements-sax-parser)
*   [5。下载源代码](#download-source-code)
*   [6。参考文献](#references)

用 Java 11 测试。

## 1。一个 XML 文件

下面是一个测试用的 XML 文件，后面所有的例子都会统计称为“staff”的 XML 元素的个数，输出是 2。

src/main/resources/staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
<company>
    <staff id="1001">
        <name>mkyong</name>
        <role>support</role>
    </staff>
    <staff id="1002">
        <name>yflow</name>
        <role>admin</role>
    </staff>
</company> 
```

## 2。统计 XML 元素(DOM 解析器)

```java
 // DOM APIs
  NodeList list = doc.getElementsByTagName("staff");
  System.out.println(list.getLength());   // 2 
```

下面是一个完整的 DOM 解析器示例，用来计算 XML 文件中“staff”元素的数量。

CountElementXmlDomParser.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.Document;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;

public class CountElementXmlDomParser {

  private static final String FILENAME = "src/main/resources/staff.xml";

  public static void main(String[] args) {

      DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

      try (InputStream is = new FileInputStream(FILENAME)) {

          DocumentBuilder db = dbf.newDocumentBuilder();

          Document doc = db.parse(is);

          // get all elements known as "staff"
          NodeList list = doc.getElementsByTagName("staff");

          System.out.println("Number of staff elements : " + list.getLength());

      } catch (ParserConfigurationException | SAXException | IOException e) {
          e.printStackTrace();
      }

  }

} 
```

输出

Terminal

```java
 Number of staff elements : 2 
```

## 3。计数 XML 元素(XPath)

```java
 XPath xpath = XPathFactory.newInstance().newXPath();
  NodeList nodes = (NodeList)
          xpath.evaluate("//staff", doc, XPathConstants.NODESET);
  int count = nodes.getLength(); 
```

下面是一个完整的 DOM 和 XPath 示例，用来计算 XML 文件中“staff”元素的数量。

CountElementXmlDomXPath.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.Document;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;

public class CountElementXmlDomXPath {

  private static final String FILENAME = "src/main/resources/staff.xml";

  public static void main(String[] args) {

      DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

      try (InputStream is = new FileInputStream(FILENAME)) {

          DocumentBuilder db = dbf.newDocumentBuilder();

          Document doc = db.parse(is);

          // get all elements known as "staff"
          // xpath
          XPath xpath = XPathFactory.newInstance().newXPath();
          NodeList nodes = (NodeList)
                  xpath.evaluate("//staff", doc, XPathConstants.NODESET);
          int count = nodes.getLength();

          System.out.println("Number of staff elements : " + count);

      } catch (ParserConfigurationException | SAXException |
                IOException | XPathExpressionException e) {
          e.printStackTrace();
      }

  }

} 
```

输出

Terminal

```java
 Number of staff elements : 2 
```

## 4。统计 XML 元素(SAX 解析器)

下面是一个完整的 SAX 解析器示例，用来计算 XML 文件中“staff”元素的数量。

4.1 我们可以创建一个 SAX 处理程序，并计算`startElement()`方法中的元素。

CountElementHandlerSax.java

```java
 package com.mkyong.xml.sax.handler;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class CountElementHandlerSax extends DefaultHandler {

  private final String elementName;
  private Integer count = 0;

  public String getElementName() {
      return elementName;
  }

  public Integer getCount() {
      return count;
  }

  public CountElementHandlerSax(String elementName) {
      this.elementName = elementName;
  }

  @Override
  public void startElement(String uri, String localName,
          String qName, Attributes attributes)
          throws SAXException {
      if (qName.equalsIgnoreCase(getElementName())) {
          count++;
      }
  }

} 
```

4.2 用上面的`CountElementHandlerSax`运行 SAX 解析器。

ReadXmlSaxParser.java

```java
 package com.mkyong.xml.sax;

import com.mkyong.xml.sax.handler.CountElementHandlerSax;
import org.xml.sax.SAXException;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.io.IOException;

public class ReadXmlSaxParser {

  private static final String FILENAME = "src/main/resources/staff.xml";

  public static void main(String[] args) {

      SAXParserFactory factory = SAXParserFactory.newInstance();

      try {

          SAXParser saxParser = factory.newSAXParser();

          // count elements name known as "staff"
          CountElementHandlerSax countStaffHandler =
                              new CountElementHandlerSax("staff");
          saxParser.parse(FILENAME, countStaffHandler);

          System.out.println("Number of staff elements : "
              + countStaffHandler.getCount());

      } catch (ParserConfigurationException | SAXException | IOException e) {
          e.printStackTrace();
      }

  }

} 
```

输出

Terminal

```java
 Number of staff elements : 2 
```

## 5。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054146/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/DOM/

$ CD src/main/Java/com/mkyong/XML/sax/

## 6。参考文献

*   [维基百科–DOM(文档对象模型)](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [维基百科–SAX(XML 简单应用编程接口)](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Simple_API_for_XML)
*   [维基百科–XPath](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/XPath)
*   [Oracle–文档对象模型](http://web.archive.org/web/20220626054146/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054146/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何在 Java 中读取 XML 文件—(SAX 解析器)](http://web.archive.org/web/20220626054146/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220626054146/https://mkyong.com/java/jaxb-hello-world-example/)
*   [如何统计 XML 文档的深度(DOM 解析器)](http://web.archive.org/web/20220626054146/https://mkyong.com/java/how-to-count-the-depth-of-xml-document-dom-example/)

<input type="hidden" id="mkyong-current-postId" value="9871">