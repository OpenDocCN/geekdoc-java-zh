# 签署 JAR 文件

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/step4.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/step4.html)

现在，您可以准备签署 JAR 文件。在命令窗口中键入以下内容，以使用密钥库条目中别名为`signFiles`的私钥签署 JAR 文件`Count.jar`，并将生成的签名 JAR 文件命名为`sCount.jar`：

```java
jarsigner -keystore examplestore -signedjar sCount.jar Count.jar signFiles 

```

您将被要求输入存储密码和私钥密码。

* * *

**注意：** `jarsigner`工具从别名为`signFiles`的密钥库条目中提取证书，并将其附加到已签名 JAR 文件的生成签名中。

* * *

建议为签名加上时间戳，如果签名未加时间戳，则会显示警告。时间戳用于验证用于签署 JAR 文件的证书在签署时是否有效。有关使用`jarsigner`选项在签名中包含时间戳的信息，请参阅签署 JAR 文件。
