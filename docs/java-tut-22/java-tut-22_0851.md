# 导出公钥证书

> 原文：[`docs.oracle.com/javase/tutorial/security/toolfilex/step4.html`](https://docs.oracle.com/javase/tutorial/security/toolfilex/step4.html)

您现在拥有一个已签名的 JAR 文件`sContract.jar`。想要使用此文件的接收方也希望验证您的签名。为此，他们需要与您用于生成签名的私钥对应的公钥。您可以通过将包含您的公钥的证书副本发送给他们来提供您的公钥。通过以下方式，将该证书从密钥库`examplestanstore`复制到名为`StanSmith.cer`的文件中：

```java
keytool -export -keystore examplestanstore
-alias signLegal -file StanSmith.cer

```

系统会提示您输入存储密码。

一旦他们获得了该证书和签名的 JAR 文件，您的接收方可以使用`jarsigner`工具来验证您的签名。请参阅合同接收方的步骤。
