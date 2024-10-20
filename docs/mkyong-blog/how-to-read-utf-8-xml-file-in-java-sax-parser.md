# 如何在 Java 中读取 UTF-8 XML 文件—(SAX 解析器)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-read-utf-8-xml-file-in-java-sax-parser/>

本文展示了如何使用 SAX 解析器读取或解析 UTF-8 XML 文件。

目录

*   [1。SAX 解析器解析 UTF-8 XML 文件。](#sax-parser-to-parse-a-utf-8-xml-file)
*   [2。XML 中的字符编码和代码](#character-encoding-in-xml-and-code)
*   [3。SAX 常见错误](#sax-common-errors)
    *   [3.1 1 字节 UTF-8 序列的无效字节 1](#invalid-byte-1-of-1-byte-utf-8-sequence)
    *   [3.2 序言中不允许出现内容](#content-is-not-allowed-in-prolog)
    *   [3.3 实体名称必须紧跟在实体引用](#the-entity-name-must-immediately-follow-the-in-the-entity-reference)中的“&
*   [4。下载源代码](#download-source-code)
*   [5。参考文献](#references)

## 1。SAX 解析器解析 UTF-8 XML 文件。

1.1 XML 文件包含 UTF-8 和中文字符。

staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
<Company>
    <staff id="1001">
        <name>揚木金</name>
        <role>support &amp; code</role>
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

1.2 下面的例子显式设置了一个`UTF-8`编码。

**注**
对于 SAX 处理程序`PrintAllHandlerSax`，参考这篇[文章](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/#read-or-parse-a-xml-file-sax)。

ReadXmlSaxParser.java

```java
 package com.mkyong.xml.sax;

import com.mkyong.xml.sax.handler.PrintAllHandlerSax;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;
import org.xml.sax.XMLReader;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.io.IOException;
import java.nio.charset.StandardCharsets;

public class ReadXmlSaxParser {

  private static final String FILENAME = "src/main/resources/staff-unicode.xml";

  public static void main(String[] args) {

      SAXParserFactory factory = SAXParserFactory.newInstance();

      try {

          SAXParser saxParser = factory.newSAXParser();

          PrintAllHandlerSax handler = new PrintAllHandlerSax();

          XMLReader xmlReader = saxParser.getXMLReader();
          xmlReader.setContentHandler(handler);

          InputSource source = new InputSource(FILENAME);

          // explicitly set a encoding
          source.setEncoding(StandardCharsets.UTF_8.displayName());

          xmlReader.parse(source);

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
Name : 揚木金
Start Element : role
End Element : role
Role : support & code
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

## 2。XML 中的字符编码和代码

确保我们使用正确的编码来解析 XML 文件。

2.1 对于 XML 文件，最好声明`encoding`属性。

```java
 <?xml version="1.0" encoding="character-encoding-here"?>
<Company>

</Company> 
```

例如，下面是一个 UTF 8 编码的 XML 文件。

```java
 <?xml version="1.0" encoding="utf-8"?>
<Company>

</Company> 
```

2.2 对于 SAX 解析器，我们可以通过`XMLReader`设置编码。

```java
 SAXParserFactory factory = SAXParserFactory.newInstance();

  try {

      SAXParser saxParser = factory.newSAXParser();

      PrintAllHandlerSax handler = new PrintAllHandlerSax();

      XMLReader xmlReader = saxParser.getXMLReader();
      xmlReader.setContentHandler(handler);

      InputSource source = new InputSource(FILENAME);

      // utf-8
      source.setEncoding(StandardCharsets.UTF_8.displayName());

      // utf-16
      // source.setEncoding(StandardCharsets.UTF_16.displayName());

      // ascii
      // source.setEncoding(StandardCharsets.US_ASCII.displayName());

      xmlReader.parse(source);

  } catch (ParserConfigurationException | SAXException | IOException e) {
      e.printStackTrace();
  } 
```

## 3。SAX 常见错误

下面是 SAX XML 解析中的一些常见错误。

### 3.1 1 字节 UTF-8 序列的无效字节 1

XML 文件包含无效的 UTF-8 字符，读[这个](http://web.archive.org/web/20220626054141/https://mkyong.com/java/sax-error-malformedbytesequenceexception-invalid-byte-1-of-1-byte-utf-8-sequence/)。

### 3.2 序言中不允许出现内容

XML 文件在 XML 声明前包含无效文本或 BOM，读[此](http://web.archive.org/web/20220626054141/https://mkyong.com/java/sax-error-content-is-not-allowed-in-prolog/)。

### 3.3 实体名称必须紧跟在实体引用
中的“&

XML 文件中的`&`是无效字符，请用`&amp;`替换或用 CDATA 括起来，例如`<![CDATA[a & b]]>`。

## 4。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054141/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/sax/

## 5。参考文献

*   [维基百科–UTF-8](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/UTF-8)
*   [UTF-8 编码表和 Unicode 字符](http://web.archive.org/web/20220626054141/https://www.utf8-chartable.de/)
*   阿帕奇·薛西斯
*   [Java SAX 解析器](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [SAX–prolog 中不允许出现内容](http://web.archive.org/web/20220626054141/https://mkyong.com/java/sax-error-content-is-not-allowed-in-prolog/)
*   [SAX–1 字节 UTF-8 序列的无效字节 1](http://web.archive.org/web/20220626054141/https://mkyong.com/java/sax-error-malformedbytesequenceexception-invalid-byte-1-of-1-byte-utf-8-sequence/)
*   [如何在 Java (SAX 解析器)中读取 XML 文件](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)

<input type="hidden" id="mkyong-current-postId" value="2431">