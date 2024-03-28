# 教程：JAXP 简介

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/intro/index.html`](https://docs.oracle.com/javase/tutorial/jaxp/intro/index.html)

Java API for XML Processing (JAXP) 是用于使用 Java 编程语言编写的应用程序处理 XML 数据的工具。JAXP 利用了解析器标准 Simple API for XML Parsing (SAX) 和 Document Object Model (DOM)，因此您可以选择将数据解析为事件流或构建对象表示。JAXP 还支持可扩展样式表语言转换 (XSLT) 标准，让您控制数据的呈现方式，并使您能够将数据转换为其他 XML 文档或其他格式，如 HTML。JAXP 还提供命名空间支持，允许您处理可能存在命名冲突的 DTD。最后，从版本 1.4 开始，JAXP 实现了流式 XML (StAX) 标准。

为了灵活性，JAXP 允许您在应用程序中使用任何符合 XML 标准的解析器。它通过所谓的可插拔层实现这一点，让您可以插入 SAX 或 DOM API 的实现。可插拔层还允许您插入 XSL 处理器，让您控制 XML 数据的显示方式。
