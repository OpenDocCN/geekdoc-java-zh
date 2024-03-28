# 表示 XML 内容

> 原文：[`docs.oracle.com/javase/tutorial/jaxb/intro/repcon.html`](https://docs.oracle.com/javase/tutorial/jaxb/intro/repcon.html)

本节描述了 JAXB 如何将 XML 内容表示为 Java 对象。

## XML 模式的 Java 表示

JAXB 支持将生成的类分组到 Java 包中。一个包包括以下内容：

+   从 XML 元素名称派生的 Java 类名称，或者由绑定自定义指定。

+   一个`ObjectFactory`类，这是一个工厂，用于返回绑定的 Java 类的实例。
