# 背景

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/properties/backgnd.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/backgnd.html)

JAXP 安全处理功能对 XML 处理器施加资源限制，以抵御某些类型的拒绝服务攻击。 但是，它并不限制获取外部资源的方式，这在尝试安全处理 XML 文档时也是有用的。 当前的 JAXP 实现支持特定于实现的属性，可用于强制执行此类限制，但需要一种标准方法来实现。

JAXP 1.5 添加了三个新属性以及它们对应的系统属性，允许用户指定可以或不可以允许的外部连接类型。属性值是协议列表。 JAXP 处理器通过将协议与列表中的协议进行匹配来检查给定的外部连接是否被允许。 如果连接在列表中，则处理器将尝试建立连接，否则将拒绝连接。

JAXP 1.5 已经集成到 7u40 和 JDK8 中。
