# 用 Java Dom 和 XSLT 打印漂亮的 XML

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/pretty-print-xml-with-java-dom-and-xslt/>

本文展示了如何使用 Java [DOM 解析器](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/) + [XSLT](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/XSLT) 来格式化或打印 XML 文档。

目录

*   [1。一个 XML 文件](#an-xml-file)
*   [2。通过转换器漂亮地打印 XML](#pretty-print-xml-via-transformer)
*   [3。通过 XSLT 打印 XML](#pretty-print-xml-via-xslt)
*   [4。下载源代码](#download-source-code)
*   [5。参考文献](#references)

*PS 用 Java 11 测试过*

## 1。一个 XML 文件

staff-simple.xml

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

## 2。通过转换器漂亮地打印 XML

在`javax.xml.transform.Transformer`中，我们可以配置`OutputKeys.INDENT`属性来打印 XML 文档。

```java
 private static void transform(Document doc, OutputStream output)
          throws TransformerException {

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      Transformer transformer = transformerFactory.newTransformer();

      // pretty print
      transformer.setOutputProperty(OutputKeys.INDENT, "yes");

      transformer.transform(new DOMSource(doc), new StreamResult(output));

  } 
```

输出

Terminal

```java
 <?xml version="1.0" encoding="utf-8" standalone="no"?>
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

但是`Transformer`会在输出中增加很多空换行符(Java 11 中测试过)，不确定为什么？

## 3。通过 XSLT 打印 XML

3.1 为了解决上述额外的空换行符问题，我们可以为漂亮打印转换添加一个`xslt`文件。

staff-format.xslt

```java
 <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output indent="yes" cdata-section-elements="cdata-other-elements"/>
    <xsl:strip-space elements="*"/>

    <xsl:template match="@*|node()">
        <xsl:copy>
            <xsl:apply-templates select="@*|node()"/>
        </xsl:copy>
    </xsl:template>

</xsl:stylesheet> 
```

3.2 XSLT 转换的 DOM 解析器示例。

XsltPrettyPrintDomParser.java

```java
 package com.mkyong.xml.dom.xslt;

import org.w3c.dom.Document;
import org.xml.sax.SAXException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.transform.stream.StreamSource;
import java.io.*;

public class XsltPrettyPrintDomParser {

  private static final String XML_FILENAME
                          = "src/main/resources/staff-simple.xml";
  private static final String XSLT_FILENAME
                          = "src/main/resources/xslt/staff-format.xslt";

  public static void main(String[] args) {

      DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

      try (InputStream is = new FileInputStream(XML_FILENAME)) {

          DocumentBuilder db = dbf.newDocumentBuilder();

          Document doc = db.parse(is);

          transform(doc, System.out);

      } catch (IOException | ParserConfigurationException |
              SAXException | TransformerException e) {
          e.printStackTrace();
      }

  }

  private static void transform(Document doc, OutputStream output)
          throws TransformerException {

      TransformerFactory transformerFactory = TransformerFactory.newInstance();

      //Transformer transformer = transformerFactory.newTransformer();

      // add XSLT for pretty print
      Transformer transformer = transformerFactory.newTransformer(
              new StreamSource(new File(XSLT_FILENAME)));

      // pretty print, this will add extra new lines
      // transformer.setOutputProperty(OutputKeys.INDENT, "yes");

      // add extra standalone to break the root node to a new line
      transformer.setOutputProperty(OutputKeys.STANDALONE, "no");

      transformer.transform(new DOMSource(doc), new StreamResult(output));

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
  </staff>
  <staff id="1002">
      <name>yflow</name>
      <role>admin</role>
  </staff>
</company> 
```

## 4。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054141/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/DOM/XSLT/

## 5。参考文献

*   [维基百科–文档对象模型](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [维基百科–XSLT](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/XSLT)
*   [Oracle–文档对象模型](http://web.archive.org/web/20220626054141/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [Java DOM 解析器 XML 和 XSLT 示例](http://web.archive.org/web/20220626054141/https://mkyong.com/java/java-dom-parser-xml-and-xslt-examples/)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何用 Java 编写 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-create-xml-file-in-java-dom/)
*   [如何在 Java 中修改 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-modify-xml-file-in-java-dom-parser/)

<input type="hidden" id="mkyong-current-postId" value="16775">