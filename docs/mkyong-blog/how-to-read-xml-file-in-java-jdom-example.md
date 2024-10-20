# 如何在 Java 中读取 XML 文件—(JDOM 解析器)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/>

[JDOM](http://web.archive.org/web/20220626054148/https://github.com/hunterhacker/jdom) 是构建在 DOM 和 SAX 之上的基于 Java 的开源 XML 解析器框架，也是 XML 文档的[文档对象模型(DOM)](http://web.archive.org/web/20220626054148/https://en.wikipedia.org/wiki/Document_Object_Model) 内存表示。

JDOM 通过提供更简单的 API 和标准的基于 Java 的集合接口，使得 XML 文档的导航更加容易。如果您不介意下载一个小库来解析 XML 文件，JDOM APIs 很容易使用。

*   [1。下载 JDOM 库](#download-the-jdom-library)
*   [2。读取或解析 XML 文件(JDOM)](#read-or-parse-an-xml-file-jdom)
*   [3。读取或解析远程 XML 文件(JDOM)](#read-or-parse-a-remote-xml-file-jdom)
*   [4。包含 XML 的字符串](#string-that-contains-xml)
*   [5。下载源代码](#download-source-code)
*   [6。参考文献](#references)

## 1。下载 JDOM 库

JDOM 不是 Java 内置 API 的一部分，我们需要下载 JDOM 库。

Maven for JDOM。

pom.xml

```java
 <dependency>
      <groupId>org.jdom</groupId>
      <artifactId>jdom2</artifactId>
      <version>2.0.6</version>
  </dependency> 
```

## 2。读取或解析 XML 文件(JDOM)

这个例子展示了如何使用 JDOM 来解析 XML 文件。

2.1 一个 XML 文件。

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

2.2 解析并打印出所有 XML 元素和节点的 JDOM 示例。

ReadXmlJDomParser.java

```java
 package com.mkyong.xml.jdom;

import org.jdom2.Document;
import org.jdom2.Element;
import org.jdom2.JDOMException;
import org.jdom2.input.SAXBuilder;

import java.io.File;
import java.io.IOException;
import java.util.List;

public class ReadXmlJDomParser {

  private static final String FILENAME = "src/main/resources/staff.xml";

  public static void main(String[] args) {

      try {

          SAXBuilder sax = new SAXBuilder();
          // XML is a local file
          Document doc = sax.build(new File(FILENAME));

          Element rootNode = doc.getRootElement();
          List<Element> list = rootNode.getChildren("staff");

          for (Element target : list) {

              String id = target.getAttributeValue("id");
              String name = target.getChildText("name");
              String role = target.getChildText("role");
              String salary = target.getChildText("salary");
              String currency = "";
              if (salary != null && salary.length() > 1) {
                  // access attribute
                  currency = target.getChild("salary").getAttributeValue("currency");
              }
              String bio = target.getChildText("bio");

              System.out.printf("Staff id : %s%n", id);
              System.out.printf("Name : %s%n", name);
              System.out.printf("Role : %s%n", role);
              System.out.printf("Salary [Currency] : %,.2f [%s]%n", Float.parseFloat(salary), currency);
              System.out.printf("Bio : %s%n%n", bio);

          }

      } catch (IOException | JDOMException e) {
          e.printStackTrace();
      }

  }
} 
```

输出

Terminal

```java
 Staff id : 1001
Name : mkyong
Role : support
Salary [Currency] : 5,000.00 [USD]
Bio : HTML tag <code>testing</code>

Staff id : 1002
Name : yflow
Role : admin
Salary [Currency] : 8,000.00 [EUR]
Bio : a & b 
```

## 3。读取或解析远程 XML 文件(JDOM)

这个示例使用 JDOM 解析器来读取或解析远程或基于网站的 XML 文件。

3.1 访问 Alexa API `https://data.alexa.com/data?cli=10&url=mkyong.com`，它会返回下面的 XML 文件。

```java
 <ALEXA VER="0.9" URL="mkyong.com/" HOME="0" AID="=" IDN="mkyong.com/">
  <SD>
    <POPULARITY URL="mkyong.com/" TEXT="20152" SOURCE="panel"/>
    <REACH RANK="14434"/>
    <RANK DELTA="+728"/>
    <COUNTRY CODE="IN" NAME="India" RANK="5322"/>
  </SD>
</ALEXA> 
```

3.2 JDOM 获取网站 Alexa 排名的例子。

ReadXmlAlexaApiJDomParser.java

```java
 package com.mkyong.xml.jdom;

import org.jdom2.Document;
import org.jdom2.Element;
import org.jdom2.JDOMException;
import org.jdom2.input.SAXBuilder;

import java.io.IOException;
import java.net.URL;
import java.util.List;

public class ReadXmlAlexaApiJDomParser {

    private static final String REMOTE_URL
        = "https://data.alexa.com/data?cli=10&url=mkyong.com";

    public static void main(String[] args) {

        try {

            SAXBuilder sax = new SAXBuilder();
            // XML is in a web-based location
            Document doc = sax.build(new URL(REMOTE_URL));

            Element rootNode = doc.getRootElement();
            List<Element> list = rootNode.getChildren("SD");

            for (Element target : list) {

                Element popularity = target.getChild("POPULARITY");

                String url = popularity.getAttributeValue("URL");
                String rank = popularity.getAttributeValue("TEXT");

                System.out.printf("URL : %s%n", url);
                System.out.printf("Alexa Rank : %s%n", rank);

            }

        } catch (IOException | JDOMException e) {
            e.printStackTrace();
        }

    }
} 
```

输出

Terminal

```java
 URL : mkyong.com/
Alexa Rank : 20152 
```

## 4。包含 XML 的字符串

这个例子展示了 JDOM 如何解析包含 XML 的`String`。

ReadXmlStringJDomParser.java

```java
 package com.mkyong.xml.jdom;

import org.jdom2.Document;
import org.jdom2.Element;
import org.jdom2.JDOMException;
import org.jdom2.input.SAXBuilder;

import java.io.IOException;
import java.io.StringReader;

public class ReadXmlStringJDomParser {

    public static void main(String[] args) {

        String XML = "<root><url>mkyong</url></root>";

        try {

            SAXBuilder sax = new SAXBuilder();
            // String that contains XML
            Document doc = sax.build(new StringReader(XML));

            Element rootNode = doc.getRootElement();
            System.out.println(rootNode.getChildText("url"));

        } catch (IOException | JDOMException e) {
            e.printStackTrace();
        }

    }
} 
```

输出

Terminal

```java
 mkyong 
```

**注**
更多 JDOM2 示例——[JDOM 2 入门](http://web.archive.org/web/20220626054148/https://github.com/hunterhacker/jdom/wiki/JDOM2-A-Primer)

## 5。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054148/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/JDOM/

## 6。参考文献

*   [JDOM 网站](http://web.archive.org/web/20220626054148/http://www.jdom.org/)
*   [JDOM2 文档](http://web.archive.org/web/20220626054148/https://github.com/hunterhacker/jdom/wiki/JDOM2-A-Primer)
*   [维基百科–JDOM](http://web.archive.org/web/20220626054148/https://en.wikipedia.org/wiki/JDOM)
*   [维基百科–用于 XML 处理的 Java API](http://web.archive.org/web/20220626054148/https://en.wikipedia.org/wiki/Java_API_for_XML_Processing)
*   [Oracle–用于 XML 处理的 Java API(JAXP)](http://web.archive.org/web/20220626054148/https://docs.oracle.com/javase/tutorial/jaxp/index.html)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054148/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何在 Java 中读取 XML 文件—(SAX 解析器)](http://web.archive.org/web/20220626054148/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220626054148/https://mkyong.com/java/jaxb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="2489">