# 合同接收方的步骤

> [`docs.oracle.com/javase/tutorial/security/toolfilex/receiver.html`](https://docs.oracle.com/javase/tutorial/security/toolfilex/receiver.html)

现在扮演接收来自 Stan 的签名 JAR 文件和证书文件的 Ruth，执行以下步骤。

1.  使用`keytool`的`-import`命令将证书导入为受信任的证书。

1.  使用`jarsigner`工具验证 JAR 文件签名。
