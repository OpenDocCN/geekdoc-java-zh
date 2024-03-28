# 课程：JAXB 简介

> 原文：[`docs.oracle.com/javase/tutorial/jaxb/intro/index.html`](https://docs.oracle.com/javase/tutorial/jaxb/intro/index.html)

Java Architecture for XML Binding (JAXB)提供了一种快速便捷的方式来绑定 XML 模式和 Java 表示，使 Java 开发人员能够轻松地在 Java 应用程序中整合 XML 数据和处理功能。作为这个过程的一部分，JAXB 提供了将 XML 实例文档解组（读取）为 Java 内容树的方法，然后将 Java 内容树重新组合（写入）为 XML 实例文档的方法。JAXB 还提供了一种从 Java 对象生成 XML 模式的方法。

JAXB 2.0 对 JAXB 1.0 进行了几项重要改进：

+   支持所有 W3C XML Schema 功能。（JAXB 1.0 没有为一些 W3C XML Schema 功能指定绑定。）

+   支持将 Java 绑定到 XML，通过添加`javax.xml.bind.annotation`包来控制此绑定。（JAXB 1.0 规定了 XML Schema 到 Java 的映射，但没有规定 Java 到 XML Schema 的映射。）

+   生成的基于模式的类数量显著减少。

+   通过 JAXP 1.3 验证 API 提供的额外验证功能。

+   更小的运行时库。

本课程描述了 JAXB 架构、功能和核心概念，并提供了使用 JAXB 的逐步示例过程。
