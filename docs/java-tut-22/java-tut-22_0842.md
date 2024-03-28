# 指定密钥库

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/wstep2.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/wstep2.html)

对于本课程，您将授予别名为 susan 的 JAR 文件中的所有代码对`C:\TestData\`目录中所有文件的读取访问权限。您需要

1.  指定包含别名为 susan 的证书信息的密钥库

1.  创建授予权限的策略条目

密钥库是在将证书导入为受信任的证书步骤中创建的名为`exampleraystore`的密钥库。

要指定密钥库，请在主策略工具窗口的**编辑**菜单中选择**更改密钥库**命令。这将弹出一个对话框，您可以在其中指定密钥库 URL 和密钥库类型。

要指定名为`exampleraystore`的密钥库，位于`C:`驱动器上的`Test`目录中，请在标记为“New KeyStore URL”的文本框中键入以下`file` URL

```java
file:/C:/Test/exampleraystore

```

如果密钥库类型是默认类型，可以将标记为“New KeyStore Type”的文本框留空，如安全属性文件中所指定的那样。您的密钥库将是默认类型，因此请将文本框留空。

* * *

**注意：** “New KeyStore URL”值是一个 URL，因此应始终使用斜杠（而不是反斜杠）作为目录分隔符。

* * *

当您完成指定密钥库 URL 后，请选择**确定**。标记为密钥库的文本框现在填入了 URL。

接下来，您需要指定新的策略条目。
