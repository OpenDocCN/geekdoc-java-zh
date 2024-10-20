# 如何在 Java 中修改 XML 文件—(DOM 解析器)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-modify-xml-file-in-java-dom-parser/>

本教程展示了如何使用 Java 内置的 DOM 解析器来修改 XML 文件。

目录

*   [1。XML 文件，](#the-xml-file-before-and-after)前后
*   [2。Dom 解析器修改 XML 文件](#dom-parser-modify-xml-file)
*   [3。下载源代码](#download-source-code)
*   [4。参考文献](#references)

用 Java 11 测试。

**注意**
请阅读 [DOM parse XML](http://web.archive.org/web/20220625195254/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/) 和 [DOM write XML](http://web.archive.org/web/20220625195254/https://mkyong.com/java/how-to-create-xml-file-in-java-dom/) 。

## 1。XML 文件，
前后

原始 XML 文件。

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

稍后我们将使用 DOM 解析器来修改以下 XML 数据。

对于员工 id `1001`

*   移除 XML 元素`name`。
*   对于 XML 元素`role`，将值更新为“founder”。

对于员工 id `1002`

*   将 XML 属性更新为`2222`。
*   添加一个新的 XML 元素`salary`，包含属性和值。
*   添加新的 XML 注释。
*   重命名一个 XML 元素，从`name`到`n`(删除和添加)。

下面是最终修改后的 XML 文件。

```java
 <?xml version="1.0" encoding="UTF-8" standalone="no"?>
<company>
    <staff id="1001">
        <role>founder</role>
    </staff>
    <staff id="2222">
        <role>admin</role>
        <salary currency="USD">1000</salary>
        <!--from name to n-->
        <n>yflow</n>
    </staff>
</company> 
```

## 2。Dom 解析器修改 XML 文件

下面是 DOM 解析器获取原始 XML 文件`staff-simple.xml`，修改 XML 并生成修改后的 XML 文件`staff-modified.xml`的例子。

ModifyXmlDomParser.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.*;
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

public class ModifyXmlDomParser {

    private static final String FILENAME = "src/main/resources/staff-simple.xml";
    // xslt for pretty print only, no special task
    private static final String FORMAT_XSLT = "src/main/resources/xslt/staff-format.xslt";

    public static void main(String[] args) {

        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

        try (InputStream is = new FileInputStream(FILENAME)) {

            DocumentBuilder db = dbf.newDocumentBuilder();

            Document doc = db.parse(is);

            NodeList listOfStaff = doc.getElementsByTagName("staff");
            //System.out.println(listOfStaff.getLength()); // 2

            for (int i = 0; i < listOfStaff.getLength(); i++) {
                // get first staff
                Node staff = listOfStaff.item(i);
                if (staff.getNodeType() == Node.ELEMENT_NODE) {
                    String id = staff.getAttributes().getNamedItem("id").getTextContent();
                    if ("1001".equals(id.trim())) {

                        NodeList childNodes = staff.getChildNodes();

                        for (int j = 0; j < childNodes.getLength(); j++) {
                            Node item = childNodes.item(j);
                            if (item.getNodeType() == Node.ELEMENT_NODE) {

                                if ("role".equalsIgnoreCase(item.getNodeName())) {
                                    // update xml element `role` text
                                    item.setTextContent("founder");
                                }
                                if ("name".equalsIgnoreCase(item.getNodeName())) {
                                    // remove xml element `name`
                                    staff.removeChild(item);
                                }
                            }

                        }

                        // add a new xml element, address
                        Element address = doc.createElement("address");
                        // add a new xml CDATA
                        CDATASection cdataSection =
                                doc.createCDATASection("HTML tag <code>testing</code>");

                        address.appendChild(cdataSection);

                        staff.appendChild(address);

                    }

                    if ("1002".equals(id.trim())) {

                        // update xml attribute, from 1002 to 2222
                        staff.getAttributes().getNamedItem("id").setTextContent("2222");

                        // add a new xml element, salary
                        Element salary = doc.createElement("salary");
                        salary.setAttribute("currency", "USD");
                        salary.appendChild(doc.createTextNode("1000"));
                        staff.appendChild(salary);

                        // rename a xml element from `name` to `n`
                        // sorry, no API for this, we need to remove and create
                        NodeList childNodes = staff.getChildNodes();

                        for (int j = 0; j < childNodes.getLength(); j++) {
                            Node item = childNodes.item(j);
                            if (item.getNodeType() == Node.ELEMENT_NODE) {

                                if ("name".equalsIgnoreCase(item.getNodeName())) {

                                    // Get the text of element `name`
                                    String name = item.getTextContent();

                                    // remove xml element `name`
                                    staff.removeChild(item);

                                    // add a new xml element, n
                                    Element n = doc.createElement("n");
                                    n.appendChild(doc.createTextNode(name));

                                    // add a new comment
                                    Comment comment = doc.createComment("from name to n");
                                    staff.appendChild(comment);

                                    staff.appendChild(n);

                                }
                            }

                        }

                    }

                }

            }

            // output to console
            // writeXml(doc, System.out);

            try (FileOutputStream output =
                         new FileOutputStream("c:\\test\\staff-modified.xml")) {
                writeXml(doc, output);
            }

        } catch (ParserConfigurationException | SAXException
                | IOException | TransformerException e) {
            e.printStackTrace();
        }

    }

    // write doc to output stream
    private static void writeXml(Document doc,
                                 OutputStream output)
            throws TransformerException, UnsupportedEncodingException {

        TransformerFactory transformerFactory = TransformerFactory.newInstance();

        // The default add many empty new line, not sure why?
        // https://mkyong.com/java/pretty-print-xml-with-java-dom-and-xslt/
        // Transformer transformer = transformerFactory.newTransformer();

        // add a xslt to remove the extra newlines
        Transformer transformer = transformerFactory.newTransformer(
                new StreamSource(new File(FORMAT_XSLT)));

        // pretty print
        transformer.setOutputProperty(OutputKeys.INDENT, "yes");
        transformer.setOutputProperty(OutputKeys.STANDALONE, "no");

        DOMSource source = new DOMSource(doc);
        StreamResult result = new StreamResult(output);

        transformer.transform(source, result);

    }

} 
```

输出–修改后的 XML 文件。

c:\\test\\staff-modified.xml

```java
 <?xml version="1.0" encoding="UTF-8" standalone="no"?>
<company>
    <staff id="1001">
        <role>founder</role>
        <address>
            <![CDATA[HTML tag <code>testing</code>]]>
        </address>
    </staff>
    <staff id="2222">
        <role>admin</role>
        <salary currency="USD">1000</salary>
        <!--from name to n-->
        <n>yflow</n>
    </staff>
</company> 
```

## 3。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220625195254/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/DOM/

## 4。参考文献

*   [维基百科–用于 XML 处理的 Java API](http://web.archive.org/web/20220625195254/https://en.wikipedia.org/wiki/Java_API_for_XML_Processing)
*   [维基百科–文档对象模型](http://web.archive.org/web/20220625195254/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [Oracle–文档对象模型](http://web.archive.org/web/20220625195254/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220625195254/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何在 Java 中读取 XML 文件—(SAX 解析器)](http://web.archive.org/web/20220625195254/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [Java DOM 解析器 XML 和 XSLT 示例](http://web.archive.org/web/20220625195254/https://mkyong.com/java/java-dom-parser-xml-and-xslt-examples/)
*   [用 Java Dom 和 XSLT 漂亮地打印 XML](http://web.archive.org/web/20220625195254/https://mkyong.com/java/pretty-print-xml-with-java-dom-and-xslt/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220625195254/https://mkyong.com/java/jaxb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="9895">