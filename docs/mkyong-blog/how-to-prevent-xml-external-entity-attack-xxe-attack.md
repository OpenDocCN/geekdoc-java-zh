# 如何防止 XML 外部实体攻击(XXE 攻击)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-prevent-xml-external-entity-attack-xxe-attack/>

这篇文章讨论了 XML 外部实体攻击(XXE 攻击)以及如何防止 XXE 从流行的 XML 解析器(如 DOM、SAX、JDOM 等)列表中删除。

*   [1。什么是 XML 外部实体攻击(XXE 攻击)](#what-is-xml-external-entity-attack-xxe-attack)
*   [2。XXE 攻击的例子](#xxe-attack-example)
*   [3。如何在 SAX 解析器中防止 XXE 攻击](#how-to-prevent-xxe-attack-in-sax-parser)
*   [4。在 XML 解析器中防止 XXE 攻击的声纳规则](#sonar-rule-to-prevent-xxe-attack-in-xml-parser)
*   [5。下载源代码](#download-source-code)
*   [6。参考文献](#references)

## 1。什么是 XML 外部实体攻击(XXE 攻击)

XML 外部实体攻击(也称为 [XXE 攻击](http://web.archive.org/web/20221230032520/https://en.wikipedia.org/wiki/XML_external_entity_attack))是针对解析包含外部实体的 XML 数据的应用程序的攻击。XML 解析器将从服务器文件系统或网络加载外部实体的内容，这可能导致任意文件泄漏或[服务器端请求伪造(SSRF)](http://web.archive.org/web/20221230032520/https://en.wikipedia.org/wiki/Server-side_request_forgery) 漏洞。

**注**
见 [CWE-611](http://web.archive.org/web/20221230032520/https://cwe.mitre.org/data/definitions/611.html)

## 2。XXE 攻击的例子

此示例显示了如何使用 XXE 攻击来检索服务器上的文件内容。

2.1 应用程序解析下面的 XML 文件，获取员工 id 并显示员工姓名。

staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
  <company>
    <staffId>1001</staffId>
  </company> 
```

输出

```java
 <?xml version="1.0" encoding="utf-8"?>
  <company>
    <staffId>Yong Mook Kim</staffId>
  </company> 
```

2.2 现在，攻击者操纵传入的 XML 文件的内容，用引用服务器文件`/etc/passwd`的恶意外部实体替换 XML 数据。

staff.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
  <company>
      <staffId>&xxe;</staffId>
  </company> 
```

XML 解析器在解析上述恶意外部实体时，会在相应位置包含文件`/etc/passwd`内容。对于弱配置的 XML 解析器(通常是默认的 XML 解析器)，`&xxe;`实体将被文件`/etc/passwd`的内容替换。

输出

```java
 <?xml version="1.0" encoding="utf-8"?>
  <company>
    <staffId>nobody:*:-2:-2:Unprivileged User:/var/empty:/usr/bin/false
    root:*:0:0:System Administrator:/var/root:/bin/sh
    daemon:*:1:1:System Services:/var/root:/usr/bin/false
    </staffId>
  </company> 
```

这里有一个易受 XXE 攻击的 SAX XML 解析器。

ReadXmlSaxParserXXE.java

```java
 package com.mkyong.xml.sax;

import com.mkyong.xml.sax.handler.PrintAllHandlerSax;
import org.xml.sax.SAXException;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.io.IOException;

public class ReadXmlSaxParserXXE {

  private static final String FILENAME = "src/main/resources/staff.xml";

  public static void main(String[] args) {

      SAXParserFactory factory = SAXParserFactory.newInstance();

      try {

          // XXE attack
          SAXParser saxParser = factory.newSAXParser();

          PrintAllHandlerSax handler = new PrintAllHandlerSax();

          saxParser.parse(FILENAME, handler);

      } catch (ParserConfigurationException | SAXException | IOException e) {
          e.printStackTrace();
      }

  }

} 
```

## 3。如何在 SAX 解析器中防止 XXE 攻击

我们可以对 SAX 解析器使用`setFeature`来完全禁用 DOCTYPE 声明。

```java
 SAXParserFactory factory = SAXParserFactory.newInstance();

  try {

      // https://rules.sonarsource.com/java/RSPEC-2755
      // prevent XXE, completely disable DOCTYPE declaration:
      factory.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);

      SAXParser saxParser = factory.newSAXParser();

      PrintAllHandlerSax handler = new PrintAllHandlerSax();

      saxParser.parse(FILENAME, handler);

  } catch (ParserConfigurationException | SAXException | IOException e) {
      e.printStackTrace();
  } 
```

现在，如果 SAX 解析器解析任何外部实体，它将提示一个错误:

输出

Terminal

```java
 org.xml.sax.SAXParseException; staffId: file:///Users/yongmookkim/projects/core-java/java-xml/src/main/resources/staff-xxe.xml;
 lineNumber: 2; columnNumber: 10; DOCTYPE is disallowed
 when the feature "http://apache.org/xml/features/disallow-doctype-decl" set to true. 
```

## 4。在 XML 解析器中防止 XXE 攻击的声纳规则

回顾一下这个 [Sonar 规则——XML 解析器不应该容易受到 XXE 攻击](http://web.archive.org/web/20221230032520/https://rules.sonarsource.com/java/RSPEC-2755),以获得流行 XML 解析器列表的完整解决方案。

对于 [DOM 解析器](http://web.archive.org/web/20221230032520/https://docs.oracle.com/en/java/javase/17/docs/api/java.xml/javax/xml/parsers/DocumentBuilderFactory.html)

```java
 DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
  // to be compliant, completely disable DOCTYPE declaration:
  factory.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
  // or completely disable external entities declarations:
  factory.setFeature("http://xml.org/sax/features/external-general-entities", false);
  factory.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
  // or prohibit the use of all protocols by external entities:
  factory.setAttribute(XMLConstants.ACCESS_EXTERNAL_DTD, "");
  factory.setAttribute(XMLConstants.ACCESS_EXTERNAL_SCHEMA, "");
  // or disable entity expansion but keep in mind that this doesn't prevent fetching external entities
  // and this solution is not correct for OpenJDK < 13 due to a bug: https://bugs.openjdk.java.net/browse/JDK-8206132
  factory.setExpandEntityReferences(false); 
```

For [SAX 解析器](http://web.archive.org/web/20221230032520/https://docs.oracle.com/en/java/javase/17/docs/api/java.xml/javax/xml/parsers/SAXParserFactory.html)

```java
 SAXParserFactory factory = SAXParserFactory.newInstance();
  // to be compliant, completely disable DOCTYPE declaration:
  factory.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
  // or completely disable external entities declarations:
  factory.setFeature("http://xml.org/sax/features/external-general-entities", false);
  factory.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
  // or prohibit the use of all protocols by external entities:
  SAXParser parser = factory.newSAXParser(); // Noncompliant
  parser.setProperty(XMLConstants.ACCESS_EXTERNAL_DTD, "");
  parser.setProperty(XMLConstants.ACCESS_EXTERNAL_SCHEMA, ""); 
```

[XMLInput](http://web.archive.org/web/20221230032520/https://docs.oracle.com/en/java/javase/17/docs/api/java.xml/javax/xml/stream/XMLInputFactory.html) ，[变压器](http://web.archive.org/web/20221230032520/https://docs.oracle.com/en/java/javase/17/docs/api/java.xml/javax/xml/transform/TransformerFactory.html)，[模式](http://web.archive.org/web/20221230032520/https://docs.oracle.com/en/java/javase/17/docs/api/java.xml/javax/xml/validation/SchemaFactory.html)

```java
 XMLInputFactory factory = XMLInputFactory.newInstance();
  // to be compliant, completely disable DOCTYPE declaration:
  factory.setProperty(XMLInputFactory.SUPPORT_DTD, false);
  // or completely disable external entities declarations:
  factory.setProperty(XMLInputFactory.IS_SUPPORTING_EXTERNAL_ENTITIES, Boolean.FALSE);
  // or prohibit the use of all protocols by external entities:
  factory.setProperty(XMLConstants.ACCESS_EXTERNAL_DTD, "");
  factory.setProperty(XMLConstants.ACCESS_EXTERNAL_SCHEMA, "");

  TransformerFactory factory = javax.xml.transform.TransformerFactory.newInstance();
  // to be compliant, prohibit the use of all protocols by external entities:
  factory.setAttribute(XMLConstants.ACCESS_EXTERNAL_DTD, "");
  factory.setAttribute(XMLConstants.ACCESS_EXTERNAL_STYLESHEET, "");

  SchemaFactory factory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
  // to be compliant, completely disable DOCTYPE declaration:
  factory.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
  // or prohibit the use of all protocols by external entities:
  factory.setProperty(XMLConstants.ACCESS_EXTERNAL_DTD, "");
  factory.setProperty(XMLConstants.ACCESS_EXTERNAL_SCHEMA, ""); 
```

对于 [Dom4j](http://web.archive.org/web/20221230032520/https://dom4j.github.io/) 库:

```java
 SAXReader xmlReader = new SAXReader();
  xmlReader.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true); 
```

对于 [JDOM](http://web.archive.org/web/20221230032520/http://www.jdom.org/) 库

```java
 SAXBuilder builder = new SAXBuilder();
  builder.setProperty(XMLConstants.ACCESS_EXTERNAL_DTD, "");
  builder.setProperty(XMLConstants.ACCESS_EXTERNAL_SCHEMA, ""); 
```

## 5。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221230032520/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/sax/

## 6。参考文献

*   [XML 解析器不应该容易受到 XXE 攻击](http://web.archive.org/web/20221230032520/https://rules.sonarsource.com/java/RSPEC-2755)
*   [维基百科–XML 外部实体攻击](http://web.archive.org/web/20221230032520/https://en.wikipedia.org/wiki/XML_external_entity_attack)
*   [如何在 Java (SAX 解析器)中读取 XML 文件](http://web.archive.org/web/20221230032520/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20221230032520/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何在 Java 中读取 XML 文件—(JDOM 解析器)](http://web.archive.org/web/20221230032520/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
*   [JAXB hello world 示例](http://web.archive.org/web/20221230032520/https://mkyong.com/java/jaxb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="17153">