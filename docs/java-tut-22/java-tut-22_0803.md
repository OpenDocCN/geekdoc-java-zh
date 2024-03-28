# StAX

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/limits/stax.html`](https://docs.oracle.com/javase/tutorial/jaxp/limits/stax.html)

StAX，JSR 173，不支持 FSP。然而，JDK 中的 StAX 实现支持新的限制属性及其对应的系统属性。这意味着，虽然没有 FSP 来开启和关闭限制，但描述的限制和系统属性的工作方式完全相同。

为了兼容性，StAX 特定属性总是在新的 JAXP 限制之前生效。例如，将`SupportDTD`属性设置为 false 会导致在输入文件包含`Entity`引用时抛出异常。因此，对于使用`SupportDTD`属性禁用 DTD 的应用程序，新限制的添加不会产生影响。
