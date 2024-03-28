# 代码接收者的步骤

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/receiver.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/receiver.html)

在这节课中，你将扮演接收者的角色，接收包含`count.class`文件的已签名的 jar 文件。它请求访问你系统资源的权限，这是它通常没有权限访问的。

这个过程需要你执行下面列出的步骤。

1.  观察受限应用程序。在导入 Susan 的证书并创建策略文件之前，该应用程序将无法访问你的系统资源。

1.  使用`keytool -import`命令将 Susan 的证书导入为受信任的证书，并将其别名设置为`susan`。

1.  设置策略文件以授予权限给由`susan`签名的`count`应用程序读取你系统上指定的文件。

1.  测试你重新配置的`count`应用程序，以验证具有受信任证书和访问你新策略文件的权限后，`count`现在可以读取你的`data`文件。
