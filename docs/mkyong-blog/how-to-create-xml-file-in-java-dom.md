# 如何用 Java 编写 XML 文件—(DOM 解析器)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-create-xml-file-in-java-dom/>

本教程展示了如何使用 Java 内置的[DOM API](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)将数据写入 XML 文件。

目录

*   [1。将 XML 写入文件](#write-xml-to-a-file)
*   [2。漂亮的打印 XML](#pretty-print-xml)
*   [3。编写 XML 元素、属性、注释 CDATA 等](#write-xml-elements-attributes-comments-cdata-etc)
*   [4。DOM 常见问题解答](#dom-faqs)
    *   [4.1 如何禁用 XML 声明？](#how-to-disable-the-xml-declaration)
    *   [4.2 如何改变 XML 编码？](#how-to-change-the-xml-encoding)
    *   4.3 如何美化 XML？
    *   [4.4 如何隐藏 XML 声明`standalone="no"`？](#how-to-hide-the-xml-declaration-standaloneno)
    *   [4.5 如何更改 XML 声明版本？](#how-to-change-the-xml-declaration-version)
*   [5。下载源代码](#download-source-code)
*   [6。参考文献](#references)

*PS 用 Java 11 测试过*

**读这个**
[如何用 Java 读取 XML 文件——(DOM 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)

## 1。将 XML 写入文件

创建 XML 并将其写入文件的步骤。

1.  创建一个`Document doc`。
2.  创建 XML 元素、属性等。，并追加到`Document doc`。
3.  创建一个`Transformer`来将`Document doc`写入一个`OutputStream`。

WriteXmlDom1.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.Document;
import org.w3c.dom.Element;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.*;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.OutputStream;

public class WriteXmlDom1 {

    public static void main(String[] args)
            throws ParserConfigurationException, TransformerException {

        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = docFactory.newDocumentBuilder();

        // root elements
        Document doc = docBuilder.newDocument();
        Element rootElement = doc.createElement("company");
        doc.appendChild(rootElement);

        doc.createElement("staff");
        rootElement.appendChild(doc.createElement("staff"));

        //...create XML elements, and others...

        // write dom document to a file
        try (FileOutputStream output =
                     new FileOutputStream("c:\\test\\staff-dom.xml")) {
            writeXml(doc, output);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    // write doc to output stream
    private static void writeXml(Document doc,
                                 OutputStream output)
            throws TransformerException {

        TransformerFactory transformerFactory = TransformerFactory.newInstance();
        Transformer transformer = transformerFactory.newTransformer();
        DOMSource source = new DOMSource(doc);
        StreamResult result = new StreamResult(output);

        transformer.transform(source, result);

    }
} 
```

输出

Terminal

```java
 <?xml version="1.0" encoding="UTF-8" standalone="no"?><company><staff>mkyong</staff></company> 
```

## 2。漂亮的打印 XML

2.1 默认情况下，`Transformer`以紧凑格式输出 XML。

Terminal

```java
 <?xml version="1.0" encoding="UTF-8" standalone="no"?><company><staff>mkyong</staff></company> 
```

2.2 我们可以设置`Transformer`中的`OutputKeys.INDENT`来启用漂亮的打印格式。

```java
 TransformerFactory transformerFactory = TransformerFactory.newInstance();
  Transformer transformer = transformerFactory.newTransformer();

  // pretty print XML
  transformer.setOutputProperty(OutputKeys.INDENT, "yes");

  DOMSource source = new DOMSource(doc);
  StreamResult result = new StreamResult(output);

  transformer.transform(source, result); 
```

输出

Terminal

```java
 <?xml version="1.0" encoding="UTF-8" standalone="no"?>
<company>
  <staff>mkyong</staff>
</company> 
```

## 3。编写 XML 元素、属性、注释 CDATA 等

下面的例子使用一个 DOM 解析器来创建 XML 并将其写入一个`OutputStream`。

WriteXmlDom3.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.CDATASection;
import org.w3c.dom.Comment;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.*;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;

public class WriteXmlDom3 {

  public static void main(String[] args)
          throws ParserConfigurationException, TransformerException {

      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();

      // root elements
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("company");
      doc.appendChild(rootElement);

      // staff 1001

      // add xml elements
      Element staff = doc.createElement("staff");
      // add staff to root
      rootElement.appendChild(staff);
      // add xml attribute
      staff.setAttribute("id", "1001");

      // alternative
      // Attr attr = doc.createAttribute("id");
      // attr.setValue("1001");
      // staff.setAttributeNode(attr);

      Element name = doc.createElement("name");
      // JDK 1.4
      //name.appendChild(doc.createTextNode("mkyong"));
      // JDK 1.5
      name.setTextContent("mkyong");
      staff.appendChild(name);

      Element role = doc.createElement("role");
      role.setTextContent("support");
      staff.appendChild(role);

      Element salary = doc.createElement("salary");
      salary.setAttribute("currency", "USD");
      salary.setTextContent("5000");
      staff.appendChild(salary);

      // add xml comment
      Comment comment = doc.createComment(
              "for special characters like < &, need CDATA");
      staff.appendChild(comment);

      Element bio = doc.createElement("bio");
      // add xml CDATA
      CDATASection cdataSection =
              doc.createCDATASection("HTML tag <code>testing</code>");
      bio.appendChild(cdataSection);
      staff.appendChild(bio);

      // staff 1002
      Element staff2 = doc.createElement("staff");
      // add staff to root
      rootElement.appendChild(staff2);
      staff2.setAttribute("id", "1002");

      Element name2 = doc.createElement("name");
      name2.setTextContent("yflow");
      staff2.appendChild(name2);

      Element role2 = doc.createElement("role");
      role2.setTextContent("admin");
      staff2.appendChild(role2);

      Element salary2 = doc.createElement("salary");
      salary2.setAttribute("currency", "EUD");
      salary2.setTextContent("8000");
      staff2.appendChild(salary2);

      Element bio2 = doc.createElement("bio");
      // add xml CDATA
      bio2.appendChild(doc.createCDATASection("a & b"));
      staff2.appendChild(bio2);

      // print XML to system console
      writeXml(doc, System.out);

  }

  // write doc to output stream
  private static void writeXml(Document doc,
                               OutputStream output)
          throws TransformerException {

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      Transformer transformer = transformerFactory.newTransformer();

      // pretty print
      transformer.setOutputProperty(OutputKeys.INDENT, "yes");

      DOMSource source = new DOMSource(doc);
      StreamResult result = new StreamResult(output);

      transformer.transform(source, result);

  }
} 
```

输出

Terminal

```java
 <?xml version="1.0" encoding="UTF-8" standalone="no"?>
<company>
  <staff id="1001">
      <name>mkyong</name>
      <role>support</role>
      <salary currency="USD">5000</salary>
      <!--for special characters like < &, need CDATA-->
      <bio>
          <![CDATA[HTML tag <code>testing</code>]]>
      </bio>
  </staff>
  <staff id="1002">
      <name>yflow</name>
      <role>admin</role>
      <salary currency="EUD">8000</salary>
      <bio>
          <![CDATA[a & b]]>
      </bio>
  </staff>
</company> 
```

## 4。DOM 常见问题解答

一些 DOM 解析器常见问题。

### 4.1 如何禁用 XML 声明？

我们可以配置`OutputKeys.OMIT_XML_DECLARATION`来显示 XML 声明。

```java
 // hide the xml declaration
  // hide <?xml version="1.0" encoding="UTF-8" standalone="no"?>
  transformer.setOutputProperty(OutputKeys.OMIT_XML_DECLARATION, "yes"); 
```

### 4.2 如何改变 XML 编码？

我们可以配置`OutputKeys.ENCODING`来改变 XML 编码。

```java
 // set xml encoding
  // <?xml version="1.0" encoding="ISO-8859-1" standalone="no"?>
  transformer.setOutputProperty(OutputKeys.ENCODING,"ISO-8859-1"); 
```

### 4.3 如何美化 XML？

我们可以配置`OutputKeys.INDENT`来启用漂亮的打印 XML。

```java
 // pretty print
  transformer.setOutputProperty(OutputKeys.INDENT, "yes"); 
```

### 如何隐藏 XML 声明` standalone="no " `？

我们可以配置`document.setXmlStandalone(true)`来隐藏 XML 声明`standalone="no"`。

```java
 TransformerFactory transformerFactory = TransformerFactory.newInstance();
  Transformer transformer = transformerFactory.newTransformer();

  // pretty print
  transformer.setOutputProperty(OutputKeys.INDENT, "yes");

  // hide the standalone="no"
  doc.setXmlStandalone(true);

  DOMSource source = new DOMSource(doc);
  StreamResult result = new StreamResult(output);

  transformer.transform(source, result); 
```

### 4.5 如何更改 XML 声明版本？

我们可以配置`document.setXmlVersion("version")`来改变 XML 声明版本。

```java
 // set xml version
  // <?xml version="1.1" encoding="ISO-8859-1" standalone="no"?>
  doc.setXmlVersion("1.1"); 
```

## 5。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054141/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/DOM/

## 6。参考文献

*   [维基百科–文档对象模型](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [Oracle–用于 XML 处理的 Java API(JAXP)](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/index.html)
*   [Oracle–文档对象模型](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [如何用 Java 编写 XML 文件—(JDOM)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-create-xml-file-in-java-jdom-parser/)
*   [如何用 Java 编写 XML 文件(StAX Writer)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-write-xml-file-in-java-stax-writer/)
*   [如何在 Java 中读取 XML 文件—(SAX 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [如何在 Java 中读取 XML 文件—(JDOM 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220626054141/https://mkyong.com/java/jaxb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="4346">