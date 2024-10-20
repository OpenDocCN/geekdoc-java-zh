# Java——将 XML 转换成属性文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-load-properties-from-xml-file/>

本文展示了如何将 XML 文件转换成属性文件。

目录

*   [1。属性–将 XML 文件转换为属性文件](#properties-convert-xml-file-to-a-properties-file)
*   [2。DOM–将 XML 文件转换成属性文件](#dom-convert-xml-file-to-a-properties-file)
*   [3。下载源代码](#download-source-code)
*   [4。参考文献](#references)

用 Java 11 测试。

## 1。属性–将 XML 文件转换为属性文件

下面的例子使用`Properties#loadFromXML()`加载一个 XML 文件并将其转换为属性值。

1.1 一个使用`properties.dtd`的 XML 文件。

server.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <comment>Server config file</comment>
    <entry key="http.port">8080</entry>
    <entry key="email.support">donot-spam-me@nospam.com</entry>
    <entry key="http.server">localhost</entry>
    <entry key="http.port">8080</entry>
</properties> 
```

1.2 我们可以使用`Properties#loadFromXML()`将 XML 文件加载到属性值中。

ConvertXmlToProperties.java

```java
 package com.mkyong.xml.tips;

import java.io.*;
import java.util.Properties;

public class ConvertXmlToProperties {

    public static void main(String[] args) throws IOException {

        Properties props = new Properties();
        try (InputStream input =
                     new FileInputStream("src/main/resources/server.xml")) {
            // loads from XML into a properties file
            props.loadFromXML(input);
        }

        try (OutputStream output =
                     new FileOutputStream("c:\\test\\server.properties")) {

            props.store(output, "");

        }

    }

} 
```

1.3 输出—`c:\\test\\server.properties`

```java
 #
#Sun May 16 16:51:31 SGT 2021
http.port=8080
email.support=donot-spam-me@nospam.com
http.server=localhost 
```

1.4 为了使`Properties#loadFromXML()`正常工作，XML 文档必须有以下 DOCTYPE 声明:

```java
 <!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd"> 
```

因为我们声明了`DOCTYPE`和`properties.dtd`，所以 XML 文档也必须遵循下面的`properties.dtd`格式。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <entry key="key1">value1</entry>
</properties> 
```

如果 XML 文件未能遵循上面在`properties.dtd`中定义的格式，JDK 会在加载 XML 文件时抛出一条错误消息。

## 2。DOM–将 XML 文件转换成属性文件

这个示例使用 DOM 解析器读取 XML 文件，并将值转换成属性文件。

2.1 一个 XML 文件。

staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
<company>
    <staff id="1001">
        <name>mkyong</name>
    </staff>
    <staff id="1002">
        <name>yflow</name>
    </staff>
</company> 
```

2.2 DOM 解析器读取 XML 并将其转换为`Properties`对象。

ConvertXmlToPropertiesDom.java

```java
 package com.mkyong.xml.tips;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import java.io.*;
import java.util.Properties;

public class ConvertXmlToPropertiesDom {

    public static void main(String[] args)
            throws IOException, ParserConfigurationException, SAXException {

        Document doc;
        Properties prop = new Properties();

        try (FileInputStream input =
                     new FileInputStream("src/main/resources/staff.xml")) {
            // convert XML file to Document
            doc = parse(input);
        }

        NodeList list = doc.getElementsByTagName("staff");

        for (int temp = 0; temp < list.getLength(); temp++) {

            Node node = list.item(temp);

            if (node.getNodeType() == Node.ELEMENT_NODE) {

                Element element = (Element) node;

                // get staff's id
                String id = element.getAttribute("id");

                // get text
                String name = element.getElementsByTagName("name")
                        .item(0).getTextContent();

                // write value to properties
                // prop does not guarantee on the order
                prop.setProperty("company.staff" + temp + ".id", id);
                prop.setProperty("company.staff" + temp + ".name", name);

            }

        }

        // write to console for testing
        prop.store(System.out, "");

        // write to a properties file
        /*try (FileOutputStream output =
                     new FileOutputStream("c:\\test\\staff.properties")) {
            prop.store(output, "");
        }*/

    }

    // get document
    private static Document parse(InputStream input)
            throws ParserConfigurationException, IOException, SAXException {

        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        DocumentBuilder db = dbf.newDocumentBuilder();
        Document doc = db.parse(input);
        return doc;

    }

} 
```

输出

Terminal

```java
 #
#Sun May 16 17:43:20 SGT 2021
company.staff0.name=mkyong
company.staff0.id=1001
company.staff1.name=yflow
company.staff1.id=1002 
```

**SAX、StAX、JDOM、JAXB 怎么样？**
上述 DOM 解析器思想适用于所有其他 XML 解析器，比如 SAX、StAX、JDOM 或 JAXB。我们使用 XML 解析器读取 XML 值，并将它们存储在一个`Properties`对象中。

## 3。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054145/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/tips/

## 4。参考文献

*   [属性 JavaDoc](http://web.archive.org/web/20220626054145/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Properties.html)
*   [Java 属性文件示例](http://web.archive.org/web/20220626054145/https://mkyong.com/java/java-properties-file-examples/)
*   [Java–将属性文件转换成 XML](http://web.archive.org/web/20220626054145/https://mkyong.com/java/how-to-store-properties-into-xml-file/)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054145/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何在 Java (SAX 解析器)中读取 XML 文件](http://web.archive.org/web/20220626054145/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [如何在 Java 中读取 XML 文件(StAX 解析器)](http://web.archive.org/web/20220626054145/https://mkyong.com/java/how-to-read-xml-file-in-java-stax-parser/)
*   [如何在 Java 中读取 XML 文件—(JDOM 解析器)](http://web.archive.org/web/20220626054145/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220626054145/https://mkyong.com/java/jaxb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="6861">