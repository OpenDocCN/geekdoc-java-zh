# 范围和顺序

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/properties/scope.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/scope.html)

`javax.xml.XMLConstants#FEATURE_SECURE_PROCESSING`（FSP）是包括 DOM、SAX、Schema Validation、XSLT 和 XPath 在内的 XML 处理器的必需功能。当设置为`true`时，建议实现启用由上述新属性定义的访问限制。为了兼容性，尽管对于 DOM、SAX 和 Schema Validation，默认情况下 FSP 为 true，但 JAXP 1.5 不会启用新的限制。

对于 JDK 8，建议将新的`accessExternal*`属性在 FSP 被明确设置时设置为空字符串。这仅在通过 API 设置 FSP 时才会发生，例如`factory.setFeature(FSP, true)`。尽管对于 DOM、SAX 和 Schema Validation，默认情况下 FSP 为 true，但 JDK 8 并不将其视为“明确”设置，因此默认情况下不会设置限制。

在`jaxp.properties`文件中指定的属性会影响 JDK 或 JRE 的所有调用，并将覆盖其默认值，或者可能已经由 FEATURE_SECURE_PROCESSING 设置的值。

当设置系统属性时，将仅影响一个调用，并将覆盖默认设置或在 jaxp.properties 中设置的设置，或者可能已经由 FEATURE_SECURE_PROCESSING 设置的设置。

通过 JAXP 工厂或`SAXParser`指定的 JAXP 属性优先于系统属性，`jaxp.properties`文件以及`javax.xml.XMLConstants#FEATURE_SECURE_PROCESSING`。

新的 JAXP 属性在以下情况下对其试图限制的相关构造没有影响：

+   当存在解析器并且解析器返回的源不为 null 时。这适用于可能设置在 SAX 和 DOM 解析器上的实体解析器，StAX 解析器上的 XML 解析器，SchemaFactory 上的 LSResourceResolver，验证器或 ValidatorHandler，或者转换器上的 URIResolver。

+   当通过调用`SchemaFactory`的`newSchema`方法显式创建模式时。

+   当不需要外部资源时。例如，以下功能/属性由参考实现支持，并可用于指示处理器不加载外部 DTD 或解析外部实体。

    ```java
    http://apache.org/xml/features/disallow-doctype-decl true
    http://apache.org/xml/features/nonvalidating/load-external-dtd false
    http://xml.org/sax/features/external-general-entities false
    http://xml.org/sax/features/external-parameter-entities false

    ```
