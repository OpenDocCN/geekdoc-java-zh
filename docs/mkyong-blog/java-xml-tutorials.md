# Java XML 教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/java-xml-tutorials/>

![java xml logo](img/9b0ed6051567b682eb9fa59a18b4060a.png)

在这一系列 Java XML 教程中，我们将展示如何使用 DOM、SAX、StAX 和 JDOM 等 XML 解析器来读写 XML 文档；另外，JAXB 将 XML 转换成对象或从对象转换成 XML。

一般来说，有两种处理 XML 文档的编程模型:DOM 和 SAX(流)。

目录

*   [1。文档对象模型](#document-object-model-dom)
*   [2。XML 的简单应用编程接口(SAX)](#simple-api-for-xml-sax)
*   [3。XML 流应用编程接口(StAX)](#streaming-api-for-xml-stax)
*   [4。第三方 XML 解析器(JDOM)](#third-party-xml-parser-jdom)
*   [5。用于 XML 绑定的 Java 架构(JAXB)](#java-architecture-for-xml-binding-jaxb)
*   [6。Java XML 常见问题解答](#java-xml-faqs)
*   [7。下载源代码](#download-source-code)
*   [8。参考文献](#references)

DOM、SAX 和 StAX 都是 Java APIs 的一部分。

## 1。文档对象模型

[文档对象模型(DOM)](http://web.archive.org/web/20221230032452/https://en.wikipedia.org/wiki/Document_Object_Model) 使用节点将整个 XML 文档表示为一个树形结构，并将它们存储在内存中。

DOM 有利于操纵小的 XML 文件，比如读、写和修改 XML 结构；DOM 不用于解析或操作大型 XML 文件，因为在内存中构建整个 XML 结构会消耗大量内存。

DOM 解析器示例

1.  [DOM–读取 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/)
2.  [DOM–编写 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-create-xml-file-in-java-dom/)
3.  [DOM–修改 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-modify-xml-file-in-java-dom-parser/)
4.  [DOM–漂亮的打印 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/pretty-print-xml-with-java-dom-and-xslt/)
5.  [DOM–XML 和 XSLT](http://web.archive.org/web/20221230032452/https://mkyong.com/java/java-dom-parser-xml-and-xslt-examples/)

## 2。XML 的简单应用编程接口(SAX)

Simple API for XML (SAX)是一个流模型、事件驱动、推送解析 API，用于读取 XML 文档(需要另一个 API 来编写)。SAX 从头到尾读取 XML 文件，当遇到一个元素时调用一个方法，或者当找到特定的文本或属性时调用另一个方法。

SAX 快速高效，比 DOM 需要更少的内存，因为 SAX 不像 DOM 那样创建 XML 数据的内部表示(树结构)。

SAX 解析器示例

1.  [SAX–读取 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-read-xml-file-in-java-sax-parser/)
2.  [SAX–读取 XML UTF-8](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-read-utf-8-xml-file-in-java-sax-parser/)

## 3。XML 流应用编程接口(StAX)

XML 流 API(StAX)是一个流模型、事件驱动、用于读写 XML 文档的拉解析 API。StAX 提供了比 SAX 更简单的编程模型和比 DOM 更高效的内存管理。

StAX 解析器示例

1.  [StAX–读取 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-read-xml-file-in-java-stax-parser/)
2.  [StAX–编写 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-write-xml-file-in-java-stax-writer/)

## 4。第三方 XML 解析器(JDOM)

DOM、SAX 和 StAX 是 Java APIs 的一部分。然而，原料药可能不适合每个人的口味。或者，我们可以使用第三方 XML 解析器:

JDOM 解析器示例

1.  [JDOM–读取 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-read-xml-file-in-java-jdom-example/)
2.  [JDOM–编写 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-create-xml-file-in-java-jdom-parser/)
3.  [JDOM–修改 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-modify-xml-file-in-java-jdom/)

## 5。用于 XML 绑定的 Java 架构(JAXB)

[Jakarta XML 绑定](http://web.archive.org/web/20221230032452/https://en.wikipedia.org/wiki/Jakarta_XML_Binding)(JAXB；以前的 Java Architecture for XML Binding)是一个 XML 绑定框架，用于在 Java 类和 XML 之间进行转换。

JAXB 示例

1.  [JAXB 历史和 hello world 示例](http://web.archive.org/web/20221230032452/https://mkyong.com/java/jaxb-hello-world-example/)

## 6。Java XML 常见问题解答

一些常见问题。

1.  [XML 外部实体(XXE)](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-prevent-xml-external-entity-attack-xxe-attack/)
2.  [将 XML 转换为属性](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-load-properties-from-xml-file/)
3.  [将属性转换为 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-store-properties-into-xml-file/)
4.  [计算 XML 元素数量](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-count-xml-elements-in-java-dom-parser/)
5.  [计算 XML 的深度](http://web.archive.org/web/20221230032452/https://mkyong.com/java/how-to-count-the-depth-of-xml-document-dom-example/)
6.  [将字符串转换成 XML](http://web.archive.org/web/20221230032452/https://mkyong.com/java/java-convert-string-to-xml/)

## 7。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221230032452/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/

## 8。参考文献

*   [维基百科-XML](http://web.archive.org/web/20221230032452/https://en.wikipedia.org/wiki/XML)
*   [维基百科–用于 XML 处理的 Java API](http://web.archive.org/web/20221230032452/https://en.wikipedia.org/wiki/Java_API_for_XML_Processing)
*   [维基百科–文档对象模型](http://web.archive.org/web/20221230032452/https://en.wikipedia.org/wiki/Document_Object_Model)
*   [维基百科–XML 的简单 API](http://web.archive.org/web/20221230032452/https://en.wikipedia.org/wiki/Simple_API_for_XML)
*   [Oracle–用于 XML 处理的 Java API(JAXP)](http://web.archive.org/web/20221230032452/https://docs.oracle.com/javase/tutorial/jaxp/index.html)
*   [Oracle–文档对象模型](http://web.archive.org/web/20221230032452/https://docs.oracle.com/javase/tutorial/jaxp/dom/index.html)
*   [Oracle–XML 简单应用编程接口(SAX)](http://web.archive.org/web/20221230032452/https://docs.oracle.com/javase/tutorial/jaxp/sax/index.html)
*   [Oracle Streaming API for XML(StAX)](http://web.archive.org/web/20221230032452/https://docs.oracle.com/javase/tutorial/jaxp/stax/index.html)
*   [StAX 简介](http://web.archive.org/web/20221230032452/https://www.xml.com/pub/a/2003/09/17/stax.html)

<input type="hidden" id="mkyong-current-postId" value="4378">