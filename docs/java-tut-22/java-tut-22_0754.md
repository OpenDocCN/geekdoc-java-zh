# 包概述

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/intro/package.html`](https://docs.oracle.com/javase/tutorial/jaxp/intro/package.html)

SAX 和 DOM API 分别由 XML-DEV 组和 W3C 定义。定义这些 API 的库如下：

+   `javax.xml.parsers`：JAXP API 提供了不同供应商的 SAX 和 DOM 解析器的通用接口。

+   `org.w3c.dom`：定义了`Document`类（DOM）以及 DOM 的所有组件的类。

+   `org.xml.sax`：定义了基本的 SAX API。

+   `javax.xml.transform`：定义了 XSLT API，让您可以将 XML 转换为其他形式。

+   `javax.xml.stream`：提供了特定于 StAX 的转换 API。

简单 XML API（SAX）是一种事件驱动、串行访问机制，逐个元素进行处理。该级别的 API 读取和写入 XML 到数据存储库或网络。对于服务器端和高性能应用程序，您需要充分了解这个级别。但对于许多应用程序，了解最基本的内容就足够了。

DOM API 通常是一个更容易使用的 API。它提供了熟悉的对象树结构。您可以使用 DOM API 来操作封装的应用程序对象的层次结构。DOM API 非常适合交互式应用程序，因为整个对象模型都存在于内存中，用户可以访问和操作它。

另一方面，构建 DOM 需要读取整个 XML 结构并将对象树保存在内存中，因此它需要更多的 CPU 和内存资源。因此，SAX API 往往更受服务器端应用程序和不需要数据的内存表示的数据过滤器的青睐。

在`javax.xml.transform`中定义的 XSLT API 允许您将 XML 数据写入文件或将其转换为其他形式。正如本教程的 XSLT 部分所示，您甚至可以与 SAX API 一起使用它来将传统数据转换为 XML。

最后，在`javax.xml.stream`中定义的 StAX API 提供了基于 Java 技术的流式、事件驱动、拉取解析的 API，用于读取和写入 XML 文档。StAX 提供了比 SAX 更简单的编程模型，比 DOM 更高效的内存管理。
