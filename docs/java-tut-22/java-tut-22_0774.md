# 介绍 XSL、XSLT 和 XPath

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/xslt/intro.html`](https://docs.oracle.com/javase/tutorial/jaxp/xslt/intro.html)

可扩展样式表语言（XSL）有三个主要子组件：

XSL-FO

格式化对象标准。迄今为止最大的子组件，此标准提供了描述字体大小、页面布局和对象呈现的机制。此子组件不包含在 JAXP 中，也不包含在本教程中。

XSLT

这是一种转换语言，允许您定义从 XML 到其他格式的转换。例如，您可以使用 XSLT 生成 HTML 或不同的 XML 结构。您甚至可以使用它生成纯文本或将信息放入其他文档格式中。（正如您将在从任意数据结构生成 XML 中看到的，一个聪明的应用程序可以利用它来操作非 XML 数据）。

XPath

归根结底，XSLT 是一种语言，让您指定在遇到特定元素时要执行的操作。但是，要为 XML 数据结构的不同部分编写程序，您需要在任何给定时间指定您正在讨论的结构部分。XPath 就是这种规范语言。它是一种寻址机制，允许您指定到元素的路径，以便例如，可以区分`<article><title>`和`<person><title>`。通过这种方式，您可以为不同的`<title>`元素描述不同类型的翻译。

本节的其余部分描述了组成 JAXP 转换 API 的包。

## JAXP 转换包

这里是组成 JAXP 转换 API 的包的描述：

`javax.xml.transform`

该包定义了您用于获取`Transformer`对象的工厂类。然后，您使用来自其他三个包中的类创建源（source）和结果（result）对象，并调用其`transform()`方法使转换发生。源和结果对象是使用其他三个包中的类创建的。

`javax.xml.transform.dom`

定义了`DOMSource`和`DOMResult`类，让您可以将 DOM 用作转换的输入或输出。

`javax.xml.transform.sax`

定义了`SAXSource`和`SAXResult`类，让您可以将 SAX 事件生成器用作转换的输入，或将 SAX 事件作为输出传递给 SAX 事件处理器。

`javax.xml.transform.stream`

定义了`StreamSource`和`StreamResult`类，让您可以将 I/O 流用作转换的输入或输出。

## XSLT 示例程序

与本教程中的其他课程不同，本课程中使用的示例程序未包含在 JAXP 1.4.2 参考实现提供的*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录中。但是您可以`在此处下载 XSLT 示例的 ZIP 文件`。
