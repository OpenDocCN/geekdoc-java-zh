# XML 的流 API

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/intro/streaming.html`](https://docs.oracle.com/javase/tutorial/jaxp/intro/streaming.html)

StAX 是 JAXP 家族中最新的 API，为那些希望进行高性能流过滤、处理和修改的开发人员提供了一种替代方案，特别是对于内存较低和扩展性要求有限的情况。

总结一下，StAX 提供了标准的、双向的**拉取解析器**接口，用于流式 XML 处理，提供了比 SAX 更简单的编程模型，比 DOM 更高效的内存管理。StAX 使开发人员能够将 XML 流解析和修改为事件，并扩展 XML 信息模型以允许特定于应用程序的添加。有关 StAX 与几种替代 API 的更详细比较，请参见 XML 流 API，以及 将 StAX 与其他 JAXP API 进行比较。

## StAX 包

StAX API 定义在 表 1-4 中所示的包中。

表 1-4 StAX 包

| 包 | 描述 |
| --- | --- |
| `javax.xml.stream` | 定义了 `XMLStreamReader` 接口，用于迭代 XML 文档的元素。`XMLStreamWriter` 接口指定了 XML 的写入方式。 |
| `javax.xml.transform.stax` | 提供了专门针对 StAX 的转换 API。 |
