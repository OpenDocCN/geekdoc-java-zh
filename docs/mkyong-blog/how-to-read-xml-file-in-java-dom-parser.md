# 如何在 Java 中读取 XML 文件—(DOM 解析器)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/>

![java xml dom parser](img/a1814cafd38193764a9c3abc51fb6ae7.png)

本教程将向您展示如何使用 Java 内置的 DOM 解析器来读取 XML 文件。

*   [1。什么是文档对象模型](#what-is-document-object-model-dom)
*   [2。读取或解析 XML 文件](#read-or-parse-a-xml-file)
*   [3。读取或解析 XML 文件(Unicode)](#read-or-parse-xml-file-unicode)
*   [4。解析 Alexa API XML 响应](#parse-alexa-api-xml-response)
*   [5。下载源代码](#download-source-code)
*   [6。参考文献](#references)

**注意**
DOM 解析器在读取大型 XML 文档时速度很慢，并且会消耗大量内存，因为它将所有节点加载到内存中进行遍历和操作。

相反，我们应该考虑 [SAX 解析器](http://web.archive.org/web/20221219211256/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)来读取大尺寸的 XML 文档，SAX 比 DOM 更快并且使用更少的内存。

## 1。什么是文档对象模型

文档对象模型(DOM)使用节点将 HTML 或 XML 文档表示为树结构。

下面是一个简单的 XML 文档:

```java
 <company>
    <staff id="1001">
        <firstname>yong</firstname>
        <lastname>mook kim</lastname>
        <nickname>mkyong</nickname>
        <salary currency="USD">100000</salary>
    </staff>
</company> 
```

DOM 常用术语。

*   `<company>`是根元素。
*   `<staff>, <firstname> and all <?>`是元素节点。
*   文本节点是由元素节点包装的值；比如`<firstname>yong</firstname>`，`yong`就是文本节点。
*   该属性是元素节点的一部分；例如，`<staff id="1001">``id`是`staff`元素的属性。

*延伸阅读*

*   [维基百科–文档对象模型(DOM)](http://web.archive.org/web/20221219211256/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [Mozilla–文档对象模型(DOM)](http://web.archive.org/web/20221219211256/https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

## 2。读取或解析 XML 文件

这个例子向您展示了如何使用 Java 内置的 DOM 解析器 API 来读取或解析 XML 文件。

2.1 查看下面的 XML 文件。

/users/mkyong/staff.xml

```java
 <?xml version="1.0"?>
<company>
    <staff id="1001">
        <firstname>yong</firstname>
        <lastname>mook kim</lastname>
        <nickname>mkyong</nickname>
        <salary currency="USD">100000</salary>
    </staff>
    <staff id="2001">
        <firstname>low</firstname>
        <lastname>yin fong</lastname>
        <nickname>fong fong</nickname>
        <salary currency="INR">200000</salary>
    </staff>
</company> 
```

下面的 2.2 是一个解析或读取上述 XML 文件的 DOM 解析器示例。

ReadXmlDomParser.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import java.io.File;
import java.io.IOException;
import java.io.InputStream;

public class ReadXmlDomParser {

  private static final String FILENAME = "/users/mkyong/staff.xml";

  public static void main(String[] args) {

      // Instantiate the Factory
      DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

      try {

          // optional, but recommended
          // process XML securely, avoid attacks like XML External Entities (XXE)
          dbf.setFeature(XMLConstants.FEATURE_SECURE_PROCESSING, true);

          // parse XML file
          DocumentBuilder db = dbf.newDocumentBuilder();

          Document doc = db.parse(new File(FILENAME));

          // optional, but recommended
          // http://stackoverflow.com/questions/13786607/normalization-in-dom-parsing-with-java-how-does-it-work
          doc.getDocumentElement().normalize();

          System.out.println("Root Element :" + doc.getDocumentElement().getNodeName());
          System.out.println("------");

          // get <staff>
          NodeList list = doc.getElementsByTagName("staff");

          for (int temp = 0; temp < list.getLength(); temp++) {

              Node node = list.item(temp);

              if (node.getNodeType() == Node.ELEMENT_NODE) {

                  Element element = (Element) node;

                  // get staff's attribute
                  String id = element.getAttribute("id");

                  // get text
                  String firstname = element.getElementsByTagName("firstname").item(0).getTextContent();
                  String lastname = element.getElementsByTagName("lastname").item(0).getTextContent();
                  String nickname = element.getElementsByTagName("nickname").item(0).getTextContent();

                  NodeList salaryNodeList = element.getElementsByTagName("salary");
                  String salary = salaryNodeList.item(0).getTextContent();

                  // get salary's attribute
                  String currency = salaryNodeList.item(0).getAttributes().getNamedItem("currency").getTextContent();

                  System.out.println("Current Element :" + node.getNodeName());
                  System.out.println("Staff Id : " + id);
                  System.out.println("First Name : " + firstname);
                  System.out.println("Last Name : " + lastname);
                  System.out.println("Nick Name : " + nickname);
                  System.out.printf("Salary [Currency] : %,.2f [%s]%n%n", Float.parseFloat(salary), currency);

              }
          }

      } catch (ParserConfigurationException | SAXException | IOException e) {
          e.printStackTrace();
      }

  }

} 
```

输出

Terminal

```java
 Root Element :company
------
Current Element :staff
Staff Id : 1001
First Name : yong
Last Name : mook kim
Nick Name : mkyong
Salary [Currency] : 100,000.00 [USD]

Current Element :staff
Staff Id : 2001
First Name : low
Last Name : yin fong
Nick Name : fong fong
Salary [Currency] : 200,000.00 [INR] 
```

## 3。读取或解析 XML 文件(Unicode)

在 DOM parser 中，读取普通 XML 文件和 Unicode XML 文件没有区别。

3.1 查看下面包含一些中文字符(Unicode)的 XML 文件。

src/main/resources/staff-unicode.xml

```java
 <?xml version="1.0"?>
<company>
    <staff id="1001">
        <firstname>揚</firstname>
        <lastname>木金</lastname>
        <nickname>mkyong</nickname>
        <salary currency="USD">100000</salary>
    </staff>
    <staff id="2001">
        <firstname>low</firstname>
        <lastname>yin fong</lastname>
        <nickname>fong fong</nickname>
        <salary currency="INR">200000</salary>
    </staff>
</company> 
```

3.2 下面的示例解析上面的 XML 文件；它逐个循环所有的节点并打印出来。

ReadXmlDomParserLoop.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.*;
import org.xml.sax.SAXException;

import javax.xml.XMLConstants;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import java.io.IOException;
import java.io.InputStream;

public class ReadXmlDomParserLoop {

  public static void main(String[] args) {

      // Instantiate the Factory
      DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

      try (InputStream is = readXmlFileIntoInputStream("staff-unicode.xml")) {

          // parse XML file
          DocumentBuilder db = dbf.newDocumentBuilder();

          // read from a project's resources folder
          Document doc = db.parse(is);

          System.out.println("Root Element :" + doc.getDocumentElement().getNodeName());
          System.out.println("------");

          if (doc.hasChildNodes()) {
              printNote(doc.getChildNodes());
          }

      } catch (ParserConfigurationException | SAXException | IOException e) {
          e.printStackTrace();
      }

  }

  private static void printNote(NodeList nodeList) {

      for (int count = 0; count < nodeList.getLength(); count++) {

          Node tempNode = nodeList.item(count);

          // make sure it's element node.
          if (tempNode.getNodeType() == Node.ELEMENT_NODE) {

              // get node name and value
              System.out.println("\nNode Name =" + tempNode.getNodeName() + " [OPEN]");
              System.out.println("Node Value =" + tempNode.getTextContent());

              if (tempNode.hasAttributes()) {

                  // get attributes names and values
                  NamedNodeMap nodeMap = tempNode.getAttributes();
                  for (int i = 0; i < nodeMap.getLength(); i++) {
                      Node node = nodeMap.item(i);
                      System.out.println("attr name : " + node.getNodeName());
                      System.out.println("attr value : " + node.getNodeValue());
                  }

              }

              if (tempNode.hasChildNodes()) {
                  // loop again if has child nodes
                  printNote(tempNode.getChildNodes());
              }

              System.out.println("Node Name =" + tempNode.getNodeName() + " [CLOSE]");

          }

      }

  }

  // read file from project resource's folder.
  private static InputStream readXmlFileIntoInputStream(final String fileName) {
      return ReadXmlDomParserLoop.class.getClassLoader().getResourceAsStream(fileName);
  }

} 
```

输出

Terminal

```java
 Root Element :company
------

Node Name =company [OPEN]
Node Value =

        揚
        木金
        mkyong
        100000

        low
        yin fong
        fong fong
        200000

Node Name =staff [OPEN]
Node Value =
        揚
        木金
        mkyong
        100000

attr name : id
attr value : 1001

Node Name =firstname [OPEN]
Node Value =揚
Node Name =firstname [CLOSE]

Node Name =lastname [OPEN]
Node Value =木金
Node Name =lastname [CLOSE]

Node Name =nickname [OPEN]
Node Value =mkyong
Node Name =nickname [CLOSE]

Node Name =salary [OPEN]
Node Value =100000
attr name : currency
attr value : USD
Node Name =salary [CLOSE]
Node Name =staff [CLOSE]

Node Name =staff [OPEN]
Node Value =
        low
        yin fong
        fong fong
        200000

attr name : id
attr value : 2001

Node Name =firstname [OPEN]
Node Value =low
Node Name =firstname [CLOSE]

Node Name =lastname [OPEN]
Node Value =yin fong
Node Name =lastname [CLOSE]

Node Name =nickname [OPEN]
Node Value =fong fong
Node Name =nickname [CLOSE]

Node Name =salary [OPEN]
Node Value =200000
attr name : currency
attr value : INR
Node Name =salary [CLOSE]
Node Name =staff [CLOSE]
Node Name =company [CLOSE] 
```

## 4。解析 Alexa API XML 响应

这个例子展示了如何使用 DOM 解析器解析来自 Alexa 的 API 的 XML 响应。

4.1 向下面的 Alexa API 发送请求。

Terminal

```java
 https://data.alexa.com/data?cli=10&url;=mkyong.com 
```

4.2 Alexa API 将返回以下 XML 响应。Alexa 排名在`POPULARITY`元素内，即`TEXT`属性。

```java
 <!--  Need more Alexa data?  Find our APIs here: https://aws.amazon.com/alexa/  -->
<ALEXA VER="0.9" URL="mkyong.com/" HOME="0" AID="=" IDN="mkyong.com/">
  <SD>
    <POPULARITY URL="mkyong.com/" TEXT="20162" SOURCE="panel"/>
    <REACH RANK="14430"/>
    <RANK DELTA="+947"/>
    <COUNTRY CODE="IN" NAME="India" RANK="4951"/>
  </SD>
</ALEXA> 
```

4.3 我们使用一个 DOM 解析器直接选择`POPULARITY`元素并打印出`TEXT`属性的值。

ReadXmlAlexaApi.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.NodeList;

import javax.xml.XMLConstants;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import java.io.InputStream;
import java.net.URL;
import java.net.URLConnection;

public class ReadXmlAlexaApi {

    private static final String ALEXA_API = "http://data.alexa.com/data?cli=10&url=";
    private final DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

    public static void main(String[] args) {

        ReadXmlAlexaApi obj = new ReadXmlAlexaApi();
        int alexaRanking = obj.getAlexaRanking("mkyong.com");

        System.out.println("Ranking: " + alexaRanking);

    }

    public int getAlexaRanking(String domain) {

        int result = 0;

        String url = ALEXA_API + domain;

        try {

            URLConnection conn = new URL(url).openConnection();

            try (InputStream is = conn.getInputStream()) {

                // unknown XML better turn on this
                dbf.setFeature(XMLConstants.FEATURE_SECURE_PROCESSING, true);

                DocumentBuilder dBuilder = dbf.newDocumentBuilder();

                Document doc = dBuilder.parse(is);

                Element element = doc.getDocumentElement();

                // find this tag "POPULARITY"
                NodeList nodeList = element.getElementsByTagName("POPULARITY");
                if (nodeList.getLength() > 0) {

                    Element elementAttribute = (Element) nodeList.item(0);
                    String ranking = elementAttribute.getAttribute("TEXT");
                    if (!"".equals(ranking)) {
                        result = Integer.parseInt(ranking);
                    }
                }
            }

        } catch (Exception e) {
            e.printStackTrace();
            throw new IllegalArgumentException("Invalid request for domain : " + domain);
        }

        return result;
    }

} 
```

域名`mkyong.com`排名`20162`。

Terminal

```java
 Ranking: 20162 
```

**注意**
更多 DOM 解析器示例—[Oracle—将 XML 数据读入 DOM](http://web.archive.org/web/20221219211256/https://docs.oracle.com/javase/tutorial/jaxp/dom/readingXML.html)

## 5。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221219211256/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/DOM/

## 6。参考文献

*   [维基百科–用于 XML 处理的 Java API](http://web.archive.org/web/20221219211256/https://en.wikipedia.org/wiki/Java_API_for_XML_Processing)
*   [维基百科–文档对象模型](http://web.archive.org/web/20221219211256/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [Oracle–用于 XML 处理的 Java API(JAXP)](http://web.archive.org/web/20221219211256/https://docs.oracle.com/javase/tutorial/jaxp/index.html)
*   [Oracle–文档对象模型](http://web.archive.org/web/20221219211256/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [OWASP–XML 外部实体(XXE)处理](http://web.archive.org/web/20221219211256/https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing)
*   stack overflow——用 java 进行 DOM 解析的规范化——它是如何工作的？
*   [如何在 Java 中获得 Alexa 排名](http://web.archive.org/web/20221219211256/https://mkyong.com/java/how-to-get-alexa-ranking-in-java/)
*   [如何在 Java 中读取 XML 文件—(SAX 解析器)](http://web.archive.org/web/20221219211256/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [如何在 Java 中读取 XML 文件—(JDOM 解析器)](http://web.archive.org/web/20221219211256/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)

<input type="hidden" id="mkyong-current-postId" value="478">