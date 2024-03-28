# 设置策略文件以授予所需的权限。

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/rstep3.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/rstep3.html)

接下来，您将使用策略工具创建一个名为**`exampleraypolicy`**的策略文件，并在其中授予来自已签名 JAR 文件的代码权限。

JAR 文件必须使用与上一步中导入到 Ray 的密钥库（`exampleraystore`）中的公钥对应的私钥进行签名。包含公钥的证书在密钥库中被别名为`susan`。我们将授予此类代码权限以读取`C:\TestData\`目录中的任何文件。

步骤如下：

1.  启动策略工具

1.  指定密钥库

1.  使用 SignedBy 别名添加策略条目

1.  保存策略文件
