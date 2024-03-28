# 其他配置实用程序

> 原文：[`docs.oracle.com/javase/tutorial/essential/environment/other.html`](https://docs.oracle.com/javase/tutorial/essential/environment/other.html)

这里是一些其他配置实用程序的摘要。

*首选项 API*允许应用程序在一个与实现相关的后备存储中存储和检索配置数据。支持异步更新，并且同一组首选项可以被多个线程甚至多个应用程序安全地更新。更多信息，请参考[首选项 API 指南](https://docs.oracle.com/javase/8/docs/technotes/guides/preferences/index.html)。

部署在*JAR 归档*中的应用程序使用*清单*来描述归档的内容。更多信息，请参考在 JAR 文件中打包程序课程。

*Java Web Start 应用程序*的配置包含在一个*JNLP 文件*中。更多信息，请参考 Java Web Start 课程。

*Java 插件小程序*的配置部分取决于用于在网页中嵌入小程序的 HTML 标签。根据小程序和浏览器，这些标签可以包括`<applet>`、`<object>`、`<embed>`和`<param>`。更多信息，请参考 Java 小程序课程。

类[`java.util.ServiceLoader`](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html)提供了一个简单的*服务提供者*设施。服务提供者是*服务*的实现一组众所周知的接口和（通常是抽象的）类。服务提供者的类通常实现服务中定义的接口并子类化类。服务提供者可以作为扩展安装（参见扩展机制）。提供者也可以通过将它们添加到类路径或通过其他特定于平台的方式来提供。
