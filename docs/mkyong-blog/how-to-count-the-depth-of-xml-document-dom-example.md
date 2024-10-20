# 如何计算 XML 文档的深度(DOM 解析器)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-count-the-depth-of-xml-document-dom-example/>

在 DOM 解析器中，我们可以使用子节点的递归循环来查找或计算 XML 文档的深度。

目录

*   [1。一个 XML 文件](#an-xml-file)
*   [2。DOM 解析器+递归循环](#dom-parser-recursive-loop)
*   [3。DOM 解析器+树遍历器](#dom-parser-tree-walker)
*   [4。下载源代码](#download-source-code)
*   [5。参考文献](#references)

用 Java 11 测试。

**注**

*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054148/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)

## 1。一个 XML 文件

下面的 XML 文件包含 3 个深度级别。

src/main/resources/staff.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
<company>                     <!-- Level 1 -->
    <staff id="1001">         <!-- Level 2 -->
        <name>mkyong</name>   <!-- Level 3 -->
        <role>support</role>  <!-- Level 3 -->
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

## 2。DOM 解析器+递归循环

下面的例子使用了一个 DOM 解析器和递归循环来查找 XML 级别的深度。

CountDepthXmlDom.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;

public class CountDepthXmlDom {

  private static final String FILENAME = "src/main/resources/staff.xml";
  private static int DEPTH_XML = 0;

  public static void main(String[] args) {

      DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

      try (InputStream is = new FileInputStream(FILENAME)) {

          DocumentBuilder db = dbf.newDocumentBuilder();

          Document doc = db.parse(is);

          // get all elements
          NodeList childNodes = doc.getChildNodes();

          printNode(childNodes, 0);

          System.out.println("Depth of XML : " + DEPTH_XML);

      } catch (ParserConfigurationException | SAXException | IOException e) {
          e.printStackTrace();
      }

  }

  // loop recursive
  private static void printNode(NodeList nodeList, int level) {
      level++;

      if (nodeList != null && nodeList.getLength() > 0) {
          for (int i = 0; i < nodeList.getLength(); i++) {

              Node node = nodeList.item(i);
              if (node.getNodeType() == Node.ELEMENT_NODE) {

                  String result = String.format(
                          "%" + level * 5 + "s : [%s]%n", node.getNodeName(), level);
                  System.out.print(result);

                  printNode(node.getChildNodes(), level);

                  // how depth is it?
                  if (level > DEPTH_XML) {
                      DEPTH_XML = level;
                  }

              }

          }
      }

  }

} 
```

输出

Terminal

```java
 company : [1]
   staff : [2]
         name : [3]
         role : [3]
       salary : [3]
          bio : [3]
   staff : [2]
         name : [3]
         role : [3]
       salary : [3]
          bio : [3]
Depth of XML : 3 
```

## 3。DOM 解析器+树遍历器

下面的例子使用 DOM 的`TreeWalker`遍历节点并找到 XML 级别的深度。

CountDepthXmlDomTreeWalker.java

```java
 package com.mkyong.xml.dom;

import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.w3c.dom.traversal.DocumentTraversal;
import org.w3c.dom.traversal.NodeFilter;
import org.w3c.dom.traversal.TreeWalker;
import org.xml.sax.SAXException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;

public class CountDepthXmlDomTreeWalker {

    private static final String FILENAME = "src/main/resources/staff.xml";
    private static int DEPTH_XML = 0;

    public static void main(String[] args) {

        int depth = countDepthXml(FILENAME);
        System.out.println("Depth of XML : " + depth);
    }

    private static int countDepthXml(String filename) {

        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

        try (InputStream is = new FileInputStream(filename)) {

            DocumentBuilder db = dbf.newDocumentBuilder();

            Document doc = db.parse(is);

            DocumentTraversal traversal = (DocumentTraversal) doc;

            // DOM tree walker
            TreeWalker walker = traversal.createTreeWalker(
                    doc.getDocumentElement(),
                    NodeFilter.SHOW_ELEMENT,
                    null,
                    true);

            traverseXmlElements(walker, 0);

        } catch (ParserConfigurationException | SAXException | IOException e) {
            e.printStackTrace();
        }

        return DEPTH_XML;

    }

    private static void traverseXmlElements(TreeWalker walker, int level) {

        level++;

        Node node = walker.getCurrentNode();

        String result = String.format(
                "%" + level * 5 + "s : [%s]%n", node.getNodeName(), level);
        System.out.print(result);

        for (Node n = walker.firstChild();
             n != null;
             n = walker.nextSibling()) {
            traverseXmlElements(walker, level);
        }

        walker.setCurrentNode(node);

        // how depth is it?
        if (level > DEPTH_XML) {
            DEPTH_XML = level;
        }

    }

} 
```

输出

Terminal

```java
 company : [1]
   staff : [2]
         name : [3]
         role : [3]
       salary : [3]
          bio : [3]
   staff : [2]
         name : [3]
         role : [3]
       salary : [3]
          bio : [3]
Depth of XML : 3 
```

## 4。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054148/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/DOM/

## 5。参考文献

*   [维基百科–文档对象模型](http://web.archive.org/web/20220626054148/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [Oracle–文档对象模型](http://web.archive.org/web/20220626054148/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [如何在 Java 中读取 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054148/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
*   [如何用 Java 编写 XML 文件—(DOM 解析器)](http://web.archive.org/web/20220626054148/https://mkyong.com/java/how-to-create-xml-file-in-java-dom/)
*   [如何在 Java 中读取 XML 文件—(SAX 解析器)](http://web.archive.org/web/20220626054148/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
*   [如何在 Java 中读取 XML 文件—(JDOM 解析器)](http://web.archive.org/web/20220626054148/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
*   [JAXB hello world 示例](http://web.archive.org/web/20220626054148/https://mkyong.com/java/jaxb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="12956">