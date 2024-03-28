# Oracle 的流式 XML 解析器实现

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/stax/parser.html`](https://docs.oracle.com/javase/tutorial/jaxp/stax/parser.html)

应用服务器 9.1 包含 Sun 微系统的 JSR 173（StAX）实现，称为 Sun Java 流式 XML 解析器（简称为流式 XML 解析器）。流式 XML 解析器是一个高速、非验证的、符合 W3C XML 1.0 和 Namespace 1.0 标准的流式 XML 拉取解析器，构建在 Xerces2 代码库之上。

在 Sun 的流式 XML 解析器实现中，Xerces2 的底层，特别是 Scanner 和相关类，已经重新设计为拉取方式。除了底层的更改外，流式 XML 解析器还包括额外的与 StAX 相关的功能和许多性能增强改进。流式 XML 解析器实现在 `appserv-ws.jar` 和 `javaee.jar` 文件中，这两个文件位于 *install_dir*`/lib/` 目录中。

JAXP 参考实现中包含了 StAX 代码示例，位于 *INSTALL_DIR*`/jaxp-`*version*`/samples/stax` 目录中，展示了 Sun 的流式 XML 解析器实现的工作原理。这些示例在 示例代码 中有描述。

在继续使用示例代码之前，有两个关于流式 XML 解析器的方面需要注意：

+   报告 CDATA 事件

+   流式 XML 解析器工厂实现

下面将讨论这些主题。

## 报告 CDATA 事件

流式 XML 解析器中实现的 `javax.xml.stream.XMLStreamReader` 不报告 CDATA 事件。如果您有一个需要接收此类事件的应用程序，请配置 `XMLInputFactory` 来设置以下特定于实现的 `report-cdata-event` 属性：

```java
XMLInputFactory factory = XMLInptuFactory.newInstance();
factory.setProperty("report-cdata-event", Boolean.TRUE);

```

## 流式 XML 解析器工厂实现

大多数应用程序不需要知道工厂实现类名。对于大多数应用程序，只需将 `javaee.jar` 和 `appserv-ws.jar` 文件添加到类路径即可，因为这两个 jar 文件在 `META-INF/services` 目录下提供了各种流式 XML 解析器属性的工厂实现类名，例如 `javax.xml.stream.XMLInputFactory`、`javax.xml.stream.XMLOutputFactory` 和 `javax.xml.stream.XMLEventFactory`，这是应用程序请求工厂实例时查找操作的第三步。有关查找机制的更多信息，请参阅 `XMLInputFactory.newInstance` 方法的 Javadoc。

但是，在某些情况下，应用程序可能希望了解工厂实现类名并显式设置属性。这些情况可能包括类路径中存在多个 JSR 173 实现，应用程序希望选择其中一个，也许是性能更好的一个，包含了关键的错误修复，或类似情况。

如果一个应用程序设置了`SystemProperty`，那么这是查找操作的第一步，因此获取工厂实例相对于其他选项来说会更快；例如：

```java
javax.xml.stream.XMLInputFactory -->
  com.sun.xml.stream.ZephyrParserFactory

javax.xml.stream.XMLOutputFactory -->
  com.sun.xml.stream.ZephyrWriterFactor

javax.xml.stream.XMLEventFactory -->
  com.sun.xml.stream.events.ZephyrEventFactory

```
