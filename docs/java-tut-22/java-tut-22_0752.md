# 教程：Java API for XML Processing (JAXP)

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/index.html`](https://docs.oracle.com/javase/tutorial/jaxp/index.html)

**Java API for XML Processing (JAXP)** 教程介绍了 Java API for XML Processing (JAXP) 1.4 技术，通过 JAXP 应用示例。

## 在阅读本教程之前

要充分利用 Java API for XML Processing (JAXP) 教程中的信息，您应该具备以下技术知识：

+   Java 编程语言及其开发环境。

+   可扩展标记语言（XML）

+   文档对象模型（DOM），由万维网联盟（W3C）DOM 工作组定义。

+   简单 XML API（SAX），由 XML-DEV 邮件列表成员合作开发。

假定具有一些 DOM 和 SAX 的先验知识。本教程详细讨论了仅特定于 JAXP API 的代码。

简要描述了 JAXP 技术，包括其目的和主要特点。

介绍了 JAXP 技术中使用的概念，即简单 XML API（SAX）：何时使用 SAX，如何解析 XML 文件，如何实现 SAX 验证，如何运行 SAX 解析器以及如何处理词法事件。提供了进一步信息的链接。

介绍了文档对象模型（DOM）使用的树结构，并展示了如何使用 DOM 函数创建节点、删除节点、更改节点内容以及遍历节点层次结构。

包括如何将文档对象模型写成 XML 文件的信息，以及如何从任意数据文件生成 DOM 以将其转换为 XML。

着重介绍了基于流的 Java 技术、事件驱动、拉取解析的 XML 读写 API。StAX 可以创建快速、相对易于编程且内存占用较小的双向 XML 解析器。

介绍了在 7u40 和 JDK8 中添加的属性。

讨论了 JAXP 实现限制，包括在 7u45 中添加的三个限制。
