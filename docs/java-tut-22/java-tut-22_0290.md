# 常见的 Java Web 启动问题

> 原文：[`docs.oracle.com/javase/tutorial/deployment/webstart/problems.html`](https://docs.oracle.com/javase/tutorial/deployment/webstart/problems.html)

本节涵盖了在开发和部署 Java Web 启动应用程序时可能遇到的一些常见问题。每个问题后面都列出了可能的原因和解决方案。

**问题：** 我的浏览器将我的应用程序的 Java 网络启动协议（JNLP）文件显示为纯文本。

很可能，您的 Web 服务器不知道 JNLP 文件的正确 MIME 类型。有关更多信息，请参阅设置 Web 服务器部分。

此外，如果您正在使用代理服务器，请确保更新文件的时间戳，以便在 Web 服务器上更新资源的情况下，代理服务器将更新其缓存。

**问题：** 当我尝试启动我的 JNLP 文件时，我收到以下错误：

```java
MissingFieldException[ The following required field is missing from the launch
  file: (<application-desc>|<applet-desc>|<installer-desc>|<component-desc>)]
        at com.sun.javaws.jnl.XMLFormat.parse(Unknown Source)
        at com.sun.javaws.jnl.LaunchDescFactory.buildDescriptor(Unknown Source)
        at com.sun.javaws.jnl.LaunchDescFactory.buildDescriptor(Unknown Source)
        at com.sun.javaws.jnl.LaunchDescFactory.buildDescriptor(Unknown Source)
        at com.sun.javaws.Main.launchApp(Unknown Source)
        at com.sun.javaws.Main.continueInSecureThread(Unknown Source)
        at com.sun.javaws.Main.run(Unknown Source)
        at java.lang.Thread.run(Unknown Source)

```

通常，当您的 XML 格式不正确时会出现此错误。您可以盯着代码直到找到问题所在，但更容易的方法是在文件上运行 XML 语法检查器。（NetBeans IDE 和 jEdit 都提供 XML 语法检查器。）

然而，此错误可能在其他情况下发生，上述情况是由另一个情况引起的，在其他情况下，这是由一个在其他情况下是格式良好的 XML 文件中的以下行引起的：

```java
<description kind="short">Demonstrates choosing the drop location in the target <code>TransferHandler</code></description>

```

错误是由非法嵌入的`code`标签引起的。
