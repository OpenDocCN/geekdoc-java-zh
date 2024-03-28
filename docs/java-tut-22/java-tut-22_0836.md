# 导出公钥证书

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/step5.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/step5.html)

现在您有一个已签名的 JAR 文件`sCount.jar`。当签名的 JAR 文件中的`Count`应用程序尝试读取文件并且策略文件授予该权限给这个已签名代码时，代码接收者（Ray）的运行时系统将需要验证签名。

为了使运行时系统能够验证签名，Ray 的密钥库需要具有与用于生成签名的私钥对应的公钥。您可以通过将证书复制到名为`Example.cer`的文件中，向 Ray 发送验证公钥的证书。通过以下方式从密钥库`examplestore`复制该证书：

```java
keytool -export -keystore examplestore -alias signFiles -file Example.cer

```

你将被提示输入存储密码。
