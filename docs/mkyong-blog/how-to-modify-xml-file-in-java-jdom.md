# 如何在 Java 中修改 XML 文件—(JDOM)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-modify-xml-file-in-java-jdom/>

本教程展示了如何使用 [JDOM](http://web.archive.org/web/20220626054141/https://github.com/hunterhacker/jdom) 来修改 XML 文件。

目录

*   [1。下载 JDOM 2.x](#download-the-jdom-2x)
*   [2。XML 文件，](#the-xml-file-before-and-after)前后
*   [3。修改 XML 文件](#modify-xml-file)
*   [4。查找并删除 XML 元素](#find-and-remove-xml-elements)
*   [5。下载源代码](#download-source-code)
*   [6。参考文献](#references)

*用 JDOM 2.0.6 测试的 PS*

**注意**
我们需要了解 JDOM 如何[解析 XML](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/) 和[编写 XML](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-create-xml-file-in-java-jdom-parser/) ，以便修改 XML 文档。

## 1。下载 JDOM 2.x

Maven for JDOM。

pom.xml

```java
 <dependency>
      <groupId>org.jdom</groupId>
      <artifactId>jdom2</artifactId>
      <version>2.0.6</version>
  </dependency> 
```

## 2。XML 文件，
前后

2.1 原始 XML 文件。

c:\\test\\staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
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

2.2 见下图 [JDOM 示例](#modify-xml-file)修改上述 XML，添加、删除和更新元素、属性、注释、CDATA 等。

对于员工，id 是 1001

1.  移除 XML 元素`role`
2.  更新 XML 元素`salary`，将属性更新为 MYR

对于员工，id 是 1002

1.  移除 xml 元素`name`
2.  添加新的 xml 元素`address`和 CDATA 内容
3.  将 xml 元素`salary`更新为 2000
4.  移除 xml 元素 CDATA

另外，添加一个新的 XML 子节点`staff`，并删除所有 XML 注释。

下面是修改后的 XML 输出。

c:\\test\\staff.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<company>
  <staff id="1001">
    <name>mkyong</name>
    <salary currency="MYR">5000</salary>
    <bio><![CDATA[HTML tag <code>testing</code>]]></bio>
  </staff>
  <staff id="1002">
    <role>admin</role>
    <salary currency="EUR">2000</salary>
    <bio>a &amp; b &amp; c</bio>
    <address><![CDATA[123 & abc]]></address>
  </staff>
  <staff id="1003" />
</company> 
```

## 3。修改 XML 文件

下面的例子使用 JDOM 解析 XML 文件上面的[，修改内容并将其写入文件。](#the-xml-file-before-and-after)

阅读代码注释，这是不言自明的。

ModifyXmlJDom.java

```java
 package com.mkyong.xml.jdom;

import org.jdom2.CDATA;
import org.jdom2.Content;
import org.jdom2.Document;
import org.jdom2.Element;
import org.jdom2.JDOMException;
import org.jdom2.input.SAXBuilder;
import org.jdom2.output.Format;
import org.jdom2.output.XMLOutputter;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.List;

public class ModifyXmlJDom {

    private static final String FILENAME = "src/main/resources/staff.xml";

    public static void main(String[] args) throws JDOMException, IOException {

        SAXBuilder sax = new SAXBuilder();
        Document doc = sax.build(new File(FILENAME));

        Element rootNode = doc.getRootElement();

        List<Element> listChildrenNode = rootNode.getChildren("staff");

        // staff = 2
        System.out.println("No of child nodes: " + listChildrenNode.size());

        // loop the elements
        for (Element staff : listChildrenNode) {

            String id = staff.getAttribute("id").getValue();

            // if staff id is 1001
            if ("1001".equals(id.trim())) {

                // remove element role
                staff.removeChild("role");

                // update xml element `salary`, update attribute to MYR
                staff.getChild("salary").setAttribute("currency", "MYR");

            }

            // if staff id is 1002
            if ("1002".equals(id.trim())) {

                // remove xml element `name`
                staff.removeChild("name");

                // add a new xml element `address` and CDATA content
                staff.addContent(new Element("address")
                        .addContent(new CDATA("123 & abc")));

                // update xml element `salary` to 2000
                staff.getChild("salary").setText("2000");

                // remove the xml element CDATA
                staff.getChild("bio").setText("a & b & c"); // now the & will escape automatically
            }

            // Java 8 to remove all XML comments
            staff.getContent().removeIf(
                    content -> content.getCType() == Content.CType.Comment);

            // remove the XML comments via iterator
            /*ListIterator<Content> iter = staff.getContent().listIterator();
            while (iter.hasNext()) {
                Content content = iter.next();
                if (content.getCType() == Content.CType.Comment) {
                    iter.remove();
                }
            }*/

        }

        // add a new XML child node, staff id 1003
        rootNode.addContent(new Element("staff").setAttribute("id", "1003"));

        // print to console for testing
        XMLOutputter xmlOutput = new XMLOutputter();
        xmlOutput.setFormat(Format.getPrettyFormat());

        // write to console
        // xmlOutput.output(doc, System.out);

        // write to a file
        try (FileOutputStream output =
                     new FileOutputStream("c:\\test\\staff-update.xml")) {
            xmlOutput.output(doc, output);
        }

    }

} 
```

输出

c:\\test\\staff-update.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<company>
  <staff id="1001">
    <name>mkyong</name>
    <salary currency="MYR">5000</salary>
    <bio><![CDATA[HTML tag <code>testing</code>]]></bio>
  </staff>
  <staff id="1002">
    <role>admin</role>
    <salary currency="EUR">2000</salary>
    <bio>a &amp; b &amp; c</bio>
    <address><![CDATA[123 & abc]]></address>
  </staff>
  <staff id="1003" />
</company> 
```

## 4。查找并删除 XML 元素

如果一个 XML 元素`name`等于`mkyong`，删除它的数据，以及整个`staff`元素。

ModifyXmlJDom4.java

```java
 package com.mkyong.xml.jdom;

import org.jdom2.CDATA;
import org.jdom2.Content;
import org.jdom2.Document;
import org.jdom2.Element;
import org.jdom2.JDOMException;
import org.jdom2.input.SAXBuilder;
import org.jdom2.output.Format;
import org.jdom2.output.XMLOutputter;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.List;

public class ModifyXmlJDom4 {

  private static final String FILENAME = "src/main/resources/staff.xml";

  public static void main(String[] args) throws JDOMException, IOException {

      SAXBuilder sax = new SAXBuilder();

      Document doc = sax.build(new File(FILENAME));

      Element rootNode = doc.getRootElement();

      List<Element> listChildrenNode = rootNode.getChildren("staff");

      // loop the elements
      for (Element staff : listChildrenNode) {

          // if name = mkyong, remove the staff node
          if ("mkyong".equals(staff.getChildText("name"))) {
              rootNode.removeContent(staff);
          }

      }

      // print to console for testing
      XMLOutputter xmlOutput = new XMLOutputter();
      xmlOutput.setFormat(Format.getPrettyFormat());

      // write to console
      xmlOutput.output(doc, System.out);

  }

} 
```

输出

Terminal

```java
 <?xml version="1.0" encoding="UTF-8"?>
<company>
  <staff id="1002">
    <name>yflow</name>
    <role>admin</role>
    <salary currency="EUR">8000</salary>
    <bio><![CDATA[a & b]]></bio>
  </staff>
</company> 
```

**注**
更多 JDOM2 示例——[JDOM 2 入门](http://web.archive.org/web/20220626054141/https://github.com/hunterhacker/jdom/wiki/JDOM2-A-Primer)

## 5。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054141/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/JDOM/

## 6。参考文献

*   [JDOM 网站](http://web.archive.org/web/20220626054141/http://www.jdom.org/)
*   [JDOM 2 文档](http://web.archive.org/web/20220626054141/https://github.com/hunterhacker/jdom/wiki/JDOM2-A-Primer)
*   [JDOM JavaDoc](http://web.archive.org/web/20220626054141/http://www.jdom.org/docs/apidocs/org/jdom2/)
*   [维基百科–JDOM](http://web.archive.org/web/20220626054141/https://en.wikipedia.org/wiki/JDOM)
*   [如何在 Java 中读取 XML 文件—(JDOM)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
*   [如何用 Java 编写 XML 文件—(JDOM)](http://web.archive.org/web/20220626054141/https://mkyong.com/java/how-to-create-xml-file-in-java-jdom-parser/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220626054141/https://mkyong.com/java/jaxb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="4355">