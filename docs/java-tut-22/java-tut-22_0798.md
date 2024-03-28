# Lesson: 处理限制

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/limits/index.html`](https://docs.oracle.com/javase/tutorial/jaxp/limits/index.html)

XML 处理有时可能是一个消耗大量内存的操作。应用程序，特别是那些接受来自不受信任来源的 XML、XSD 和 XSL 的应用程序，应该通过使用 JDK 提供的 JAXP 处理限制来防范过度的内存消耗。

开发人员应该评估他们应用程序的需求和运行环境，以确定系统配置的可接受限制，并相应地设置这些限制。与大小相关的限制可用于防止处理畸形的 XML 源时消耗大量内存，而`EntityExpansionLimit`将允许应用程序在可接受水平下控制内存消耗。

在本教程中，您将了解这些限制，并学习如何正确使用它们。
