# 如何在 Java 中读取 XML 文件(SAX 解析器)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/>

本教程将向您展示如何使用 Java 内置的 SAX 解析器来读取和解析 XML 文件。

*   [1。什么是 XML 的简单 API(SAX)](#what-is-simple-api-for-xml-sax)
*   [2。读取或解析 XML 文件(SAX)](#read-or-parse-a-xml-file-sax)
*   [3。将 XML 文件转换成对象](#convert-an-xml-file-to-an-object)
*   [4。SAX 错误处理程序](#sax-error-handler)
*   [5。SAX 和 Unicode](#sax-and-unicode)
*   [6。下载源代码](#download-source-code)
*   [7。参考文献](#references)

## 1。什么是 XML 的简单 API(SAX)

1.1[XML 的简单 API(SAX)](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/sax/index.html)是一个推送 API，一个[观察者模式](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/Observer_pattern)，事件驱动，顺序地串行访问 XML 文件元素。这个 SAX 解析器从头到尾读取 XML 文件，在遇到一个元素时调用一个方法，或者在找到特定的文本或属性时调用不同的方法。

SAX 快速高效，比 DOM 需要更少的内存，因为 SAX 不像 DOM 那样创建 XML 数据的内部表示(树结构)。

**注意**
SAX 解析器比 DOM 解析器更快，使用的内存更少。SAX 适合顺序读取 XML 元素；DOM 适合于 XML 操作，比如创建、修改或删除 XML 元素。

1.2 一些常见的 SAX 事件:

*   `startDocument()`和`endDocument()`–在 XML 文档的开头和结尾调用的方法。
*   `startElement()`和`endElement()`–在 XML 元素的开头和结尾调用的方法。
*   `characters()`–用 XML 元素开始和结束之间的文本内容调用的方法。

1.3 下面是一个简单的 XML 文件。

```java
 <name>mkyong</name> 
```

SAX 解析器读取上面的 XML 文件，并依次调用以下事件或方法:

1.  `startDocument()`
2.  `startElement()`–`<name>`
3.  `characters()`–`mkyong`
4.  `endElement()`–`</name>`
5.  `endDocument()`

## 2。读取或解析 XML 文件(SAX)

这个例子向您展示了如何使用 Java 内置的 SAX 解析器 API 来读取或解析 XML 文件。

下面是一个 XML 文件。

src/main/resources/staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
<Company>
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
</Company> 
```

*P.S 在 XML 文件中，对于像`<`或者`&`这样的特殊字符，我们需要用`CDATA`将其包裹起来。*

2.2 创建一个类来扩展`org.xml.sax.helpers.DefaultHandler`，并覆盖`startElement`、`endElement`和`characters`方法来打印所有的 XML 元素、属性、注释和文本。

PrintAllHandlerSax.java

```java
 package com.mkyong.xml.sax.handler;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class PrintAllHandlerSax extends DefaultHandler {

  private StringBuilder currentValue = new StringBuilder();

  @Override
  public void startDocument() {
      System.out.println("Start Document");
  }

  @Override
  public void endDocument() {
      System.out.println("End Document");
  }

  @Override
  public void startElement(
          String uri,
          String localName,
          String qName,
          Attributes attributes) {

      // reset the tag value
      currentValue.setLength(0);

      System.out.printf("Start Element : %s%n", qName);

      if (qName.equalsIgnoreCase("staff")) {
          // get tag's attribute by name
          String id = attributes.getValue("id");
          System.out.printf("Staff id : %s%n", id);
      }

      if (qName.equalsIgnoreCase("salary")) {
          // get tag's attribute by index, 0 = first attribute
          String currency = attributes.getValue(0);
          System.out.printf("Currency :%s%n", currency);
      }

  }

  @Override
  public void endElement(String uri,
                         String localName,
                         String qName) {

      System.out.printf("End Element : %s%n", qName);

      if (qName.equalsIgnoreCase("name")) {
          System.out.printf("Name : %s%n", currentValue.toString());
      }

      if (qName.equalsIgnoreCase("role")) {
          System.out.printf("Role : %s%n", currentValue.toString());
      }

      if (qName.equalsIgnoreCase("salary")) {
          System.out.printf("Salary : %s%n", currentValue.toString());
      }

      if (qName.equalsIgnoreCase("bio")) {
          System.out.printf("Bio : %s%n", currentValue.toString());
      }

  }

  // http://www.saxproject.org/apidoc/org/xml/sax/ContentHandler.html#characters%28char%5B%5D,%20int,%20int%29
  // SAX parsers may return all contiguous character data in a single chunk,
  // or they may split it into several chunks
  @Override
  public void characters(char ch[], int start, int length) {

      // The characters() method can be called multiple times for a single text node.
      // Some values may missing if assign to a new string

      // avoid doing this
      // value = new String(ch, start, length);

      // better append it, works for single or multiple calls
      currentValue.append(ch, start, length);

  }

} 
```

2.3 `SAXParser`解析 XML 文件。

ReadXmlSaxParser.java

```java
 package com.mkyong.xml.sax;

import com.mkyong.xml.sax.handler.PrintAllHandlerSax;
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

            PrintAllHandlerSax handler = new PrintAllHandlerSax();
            saxParser.parse(FILENAME, handler);

        } catch (ParserConfigurationException | SAXException | IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

Terminal

```java
 Start Document
Start Element : Company
Start Element : staff
Staff id : 1001
Start Element : name
End Element : name
Name : mkyong
Start Element : role
End Element : role
Role : support
Start Element : salary
Currency :USD
End Element : salary
Salary : 5000
Start Element : bio
End Element : bio
Bio : HTML tag <code>testing</code>
End Element : staff
Start Element : staff
Staff id : 1002
Start Element : name
End Element : name
Name : yflow
Start Element : role
End Element : role
Role : admin
Start Element : salary
Currency :EUR
End Element : salary
Salary : 8000
Start Element : bio
End Element : bio
Bio : a & b
End Element : staff
End Element : Company
End Document 
```

## 3。将 XML 文件转换成对象

这个例子解析一个 XML 文件并把它转换成一个对象的`List`。它有效，但不推荐，试试 [JAXB](http://web.archive.org/web/20220626054141/https://mkyong.com/java/jaxb-hello-world-example/)

3.1 检查同一个 XML 文件。

src/main/resources/staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
<Company>
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
</Company> 
```

3.2 我们想将上面的 XML 文件转换成下面的`Staff`对象。

Staff.java

```java
 package com.mkyong.xml.sax.model;

import java.math.BigDecimal;

public class Staff {

  private Long id;
  private String name;
  private String role;
  private BigDecimal salary;
  private String Currency;
  private String bio;

  //... getters, setters...toString
} 
```

3.3 下面的类将完成 XML 到对象的转换。

MapStaffObjectHandlerSax.java

```java
 package com.mkyong.xml.sax.handler;

import com.mkyong.xml.sax.model.Staff;
import org.xml.sax.Attributes;
import org.xml.sax.helpers.DefaultHandler;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

public class MapStaffObjectHandlerSax extends DefaultHandler {

    private StringBuilder currentValue = new StringBuilder();
    List<Staff> result;
    Staff currentStaff;

    public List<Staff> getResult() {
        return result;
    }

    @Override
    public void startDocument() {
        result = new ArrayList<>();
    }

    @Override
    public void startElement(
            String uri,
            String localName,
            String qName,
            Attributes attributes) {

        // reset the tag value
        currentValue.setLength(0);

        // start of loop
        if (qName.equalsIgnoreCase("staff")) {

            // new staff
            currentStaff = new Staff();

            // staff id
            String id = attributes.getValue("id");
            currentStaff.setId(Long.valueOf(id));
        }

        if (qName.equalsIgnoreCase("salary")) {
            // salary currency
            String currency = attributes.getValue("currency");
            currentStaff.setCurrency(currency);
        }

    }

    public void endElement(String uri,
                           String localName,
                           String qName) {

        if (qName.equalsIgnoreCase("name")) {
            currentStaff.setName(currentValue.toString());
        }

        if (qName.equalsIgnoreCase("role")) {
            currentStaff.setRole(currentValue.toString());
        }

        if (qName.equalsIgnoreCase("salary")) {
            currentStaff.setSalary(new BigDecimal(currentValue.toString()));
        }

        if (qName.equalsIgnoreCase("bio")) {
            currentStaff.setBio(currentValue.toString());
        }

        // end of loop
        if (qName.equalsIgnoreCase("staff")) {
            result.add(currentStaff);
        }

    }

    public void characters(char ch[], int start, int length) {
        currentValue.append(ch, start, length);

    }

} 
```

3.4 运行它。

ReadXmlSaxParser2.java

```java
 package com.mkyong.xml.sax;

import com.mkyong.xml.sax.handler.MapStaffObjectHandlerSax;
import com.mkyong.xml.sax.model.Staff;
import org.xml.sax.SAXException;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class ReadXmlSaxParser2 {

    public static void main(String[] args) {

        SAXParserFactory factory = SAXParserFactory.newInstance();

        try (InputStream is = getXMLFileAsStream()) {

            SAXParser saxParser = factory.newSAXParser();

            // parse XML and map to object, it works, but not recommend, try JAXB
            MapStaffObjectHandlerSax handler = new MapStaffObjectHandlerSax();

            saxParser.parse(is, handler);

            // print all
            List<Staff> result = handler.getResult();
            result.forEach(System.out::println);

        } catch (ParserConfigurationException | SAXException | IOException e) {
            e.printStackTrace();
        }

    }

    // get XML file from resources folder.
    private static InputStream getXMLFileAsStream() {
        return ReadXmlSaxParser2.class.getClassLoader().getResourceAsStream("staff.xml");
    }

} 
```

输出

Terminal

```java
 Staff{id=1001, name='揚木金', role='support', salary=5000, Currency='USD', bio='HTML tag <code>testing</code>'}
Staff{id=1002, name='yflow', role='admin', salary=8000, Currency='EUR', bio='a & b'} 
```

## 4。SAX 错误处理程序

这个例子展示了如何为 SAX 解析器注册一个定制的错误处理程序。

4.1 创建一个类并扩展`org.xml.sax.ErrorHandler`。阅读代码进行自我解释。它只是包装了原始错误消息。

CustomErrorHandlerSax.java

```java
 package com.mkyong.xml.sax.handler;

import org.xml.sax.ErrorHandler;
import org.xml.sax.SAXException;
import org.xml.sax.SAXParseException;

import java.io.PrintStream;

public class CustomErrorHandlerSax implements ErrorHandler {

    private PrintStream out;

    public CustomErrorHandlerSax(PrintStream out) {
        this.out = out;
    }

    private String getParseExceptionInfo(SAXParseException spe) {
        String systemId = spe.getSystemId();

        if (systemId == null) {
            systemId = "null";
        }

        String info = "URI=" + systemId + " Line="
                + spe.getLineNumber() + ": " + spe.getMessage();

        return info;
    }

    public void warning(SAXParseException spe) throws SAXException {
        out.println("Warning: " + getParseExceptionInfo(spe));
    }

    public void error(SAXParseException spe) throws SAXException {
        String message = "Error: " + getParseExceptionInfo(spe);
        throw new SAXException(message);
    }

    public void fatalError(SAXParseException spe) throws SAXException {
        String message = "Fatal Error: " + getParseExceptionInfo(spe);
        throw new SAXException(message);
    }

} 
```

4.2 我们使用`saxParser.getXMLReader()`来获得`org.xml.sax.XMLReader`，它提供了更多配置 SAX 解析器的选项。

ReadXmlSaxParser3.java

```java
 package com.mkyong.xml.sax;

import com.mkyong.xml.sax.handler.CustomErrorHandlerSax;
import com.mkyong.xml.sax.handler.MapStaffObjectHandlerSax;
import com.mkyong.xml.sax.model.Staff;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;
import org.xml.sax.XMLReader;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class ReadXmlSaxParser3 {

  public static void main(String[] args) {

      SAXParserFactory factory = SAXParserFactory.newInstance();

      try (InputStream is = getXMLFileAsStream()) {

          SAXParser saxParser = factory.newSAXParser();

          // parse XML and map to object, it works, but not recommend, try JAXB
          MapStaffObjectHandlerSax handler = new MapStaffObjectHandlerSax();

          // try XMLReader
          //saxParser.parse(is, handler);

          // more options for configuration
          XMLReader xmlReader = saxParser.getXMLReader();

          // set our custom error handler
          xmlReader.setErrorHandler(new CustomErrorHandlerSax(System.err));

          xmlReader.setContentHandler(handler);

          InputSource source = new InputSource(is);

          xmlReader.parse(source);

          // print all
          List<Staff> result = handler.getResult();
          result.forEach(System.out::println);

      } catch (ParserConfigurationException | SAXException | IOException e) {
          e.printStackTrace();
      }

  }

  // get XML file from resources folder.
  private static InputStream getXMLFileAsStream() {
      return ReadXmlSaxParser2.class.getClassLoader().getResourceAsStream("staff.xml");
  }

} 
```

4.3 更新`staff.xml`，去掉`bio`元素中的`CDATA`，放一个`&`，SAX 解析器会报错。

src/main/resources/staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
<Company>
    <staff id="1001">
        <name>mkyong</name>
        <role>support</role>
        <salary currency="USD">5000</salary>
        <!-- for special characters like < &, need CDATA -->
        <bio>&</bio>
    </staff>
</Company> 
```

4.4 用上面的自定义错误处理程序运行它。

```java
 xmlReader.setErrorHandler(new CustomErrorHandlerSax(System.err)); 
```

输出

Terminal

```java
 org.xml.sax.SAXException: Fatal Error: URI=null Line=8: The entity name must immediately follow the '&' in the entity reference.
at com.mkyong.xml.sax.handler.CustomErrorHandlerSax.fatalError(CustomErrorHandlerSax.java:41)
at java.xml/com.sun.org.apache.xerces.internal.util.ErrorHandlerWrapper.fatalError(ErrorHandlerWrapper.java:181)
at java.xml/com.sun.org.apache.xerces.internal.impl.XMLErrorReporter.reportError(XMLErrorReporter.java:400)
at java.xml/com.sun.org.apache.xerces.internal.impl.XMLErrorReporter.reportError(XMLErrorReporter.java:327)
at java.xml/com.sun.org.apache.xerces.internal.impl.XMLScanner.reportFatalError(XMLScanner.java:1471)
//... 
```

4.5 在没有自定义错误处理程序的情况下运行它。

```java
 // xmlReader.setErrorHandler(new CustomErrorHandlerSax(System.err)); 
```

输出

Terminal

```java
 [Fatal Error] :8:15: The entity name must immediately follow the '&' in the entity reference.
org.xml.sax.SAXParseException; lineNumber: 8; columnNumber: 15; The entity name must immediately follow the '&' in the entity reference.
	at java.xml/com.sun.org.apache.xerces.internal.parsers.AbstractSAXParser.parse(AbstractSAXParser.java:1243)
	at java.xml/com.sun.org.apache.xerces.internal.jaxp.SAXParserImpl$JAXPSAXParser.parse(SAXParserImpl.java:635)
	at com.mkyong.xml.sax.ReadXmlSaxParser2.main(ReadXmlSaxParser2.java:44) 
```

## 5。SAX 和 Unicode

对于包含 Unicode 字符的 XML 文件，默认情况下，SAX 可以遵循 XML 编码(默认的 UTF-8)并正确解析内容。

5.1 我们可以在 XML 文件的顶部定义编码，`encoding="encoding-code"`；例如，下面是一个使用`UTF-8`编码的 XML 文件。

```java
 <?xml version="1.0" encoding="utf-8"?>
<Company>
    <staff id="1001">
        <name>揚木金</name>
        <role>support</role>
        <salary currency="USD">5000</salary>
        <bio><![CDATA[HTML tag <code>testing</code>]]></bio>
    </staff>
    <staff id="1002">
        <name>yflow</name>
        <role>admin</role>
        <salary currency="EUR">8000</salary>
        <bio><![CDATA[a & b]]></bio>
    </staff>
</Company> 
```

5.2 或者，我们可以在`InputSource`中定义一个指定的编码。

```java
 XMLReader xmlReader = saxParser.getXMLReader();
  xmlReader.setContentHandler(handler);

  InputSource source = new InputSource(is);

  // set encoding
  source.setEncoding(StandardCharsets.UTF_8.toString());

  //source.setEncoding(StandardCharsets.UTF_16.toString());

  xmlReader.parse(source); 
```

**注意**
更多 SAX 解析器示例—[Oracle—简单 XML API(SAX)](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/sax/index.html)

## 6。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054141/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/sax/

## 7。参考文献

*   [维基百科–用于 XML 处理的 Java API](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/Java_API_for_XML_Processing)
*   [维基百科–XML 的简单 API](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/Simple_API_for_XML)
*   [维基百科–文档对象模型](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [维基百科–观察者模式](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/Observer_pattern)
*   [Oracle–用于 XML 处理的 Java API(JAXP)](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/index.html)
*   [Oracle–XML 简单应用编程接口(SAX)](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/sax/index.html)
*   [Oracle–文档对象模型(DOM)](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何在 Java 中读取 XML 文件—(JDOM 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220626054141/https://mkyong.com/java/jaxb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="486">