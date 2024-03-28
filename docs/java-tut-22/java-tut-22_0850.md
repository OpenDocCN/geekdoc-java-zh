# 签署 JAR 文件

> 原文：[`docs.oracle.com/javase/tutorial/security/toolfilex/step3.html`](https://docs.oracle.com/javase/tutorial/security/toolfilex/step3.html)

现在，您已经准备好签署 JAR 文件了。

在命令窗口中一行输入以下内容，以使用密钥库条目中别名为`signLegal`的私钥签署 JAR 文件`Contract.jar`，并将生成的签名附加到命名为`sContract.jar`的结果签名的 JAR 文件中：

```java
jarsigner -keystore examplestanstore
    -signedjar sContract.jar
    Contract.jar signLegal

```

系统会提示您输入存储密码和私钥密码。

`jarsigner`工具从别名为`signLegal`的密钥库条目中提取证书，并将其附加到已签名 JAR 文件的生成签名中。
