# StAX

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/properties/stax.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/stax.html)

StAX、JSR 173 的规范尚不支持新属性。然而，在 JAXP 的上下文中，StAX 确实包括对这些属性的支持。设置新属性类似于 SAX 或 DOM，但通过 XMLInputFactory，如下所示：

```java
XMLInputFactory xif = XMLInputFactory.newInstance();
xif.setProperty("http://javax.xml.XMLConstants/property/accessExternalDTD", "file");

```

存在于 StAX、JSR 173 规范中指定的属性和特性将优先于新的 JAXP 属性。例如，当 SupportDTD 属性设置为 false 时，将导致程序在输入文件包含 DTD 之前无法解析时抛出异常。对于使用 SupportDTD 属性禁用 DTD 的应用程序，新属性的添加不会产生影响。
