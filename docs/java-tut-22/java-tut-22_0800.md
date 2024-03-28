# 范围和顺序

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/limits/scope.html`](https://docs.oracle.com/javase/tutorial/jaxp/limits/scope.html)

`javax.xml.XMLConstants#FEATURE_SECURE_PROCESSING`（FSP）功能对包括 DOM、SAX、模式验证、XSLT 和 XPath 在内的 XML 处理器是必需的。当 FSP 设置为`true`时，建议的默认限制将被强制执行。将 FSP 设置为`false`不会改变这些限制。

当 Java 安全管理器存在时，FSP 被设置为 true 且无法关闭。因此，建议的默认限制将被强制执行。

在`jaxp.properties`文件中指定的属性会影响 JDK 和 JRE 的所有调用，并将覆盖它们的默认值，或者可能已被 FSP 设置的值。

系统属性在设置时会影响 JDK 和 JRE 的调用，并覆盖默认设置或者在`jaxp.properties`中设置的值，或者可能已被 FSP 设置的值。

通过 JAXP 工厂或`SAXParser`指定的 JAXP 属性优先于系统属性，`jaxp.properties`文件以及`FEATURE_SECURE_PROCESSING`。
