# 将证书导入为受信任的证书

> 原文：[`docs.oracle.com/javase/tutorial/security/toolfilex/rstep1.html`](https://docs.oracle.com/javase/tutorial/security/toolfilex/rstep1.html)

假设您是 Ruth，并已从 Stan Smith 那里收到

+   签名的 JAR 文件`sContract.jar`包含一个合同

+   文件`StanSmith.cer`包含与用于签署 JAR 文件的私钥对应的公钥证书

在您可以使用`jarsigner`工具检查 JAR 文件签名的真实性之前，您需要将 Stan 的证书导入您的密钥库。

即使您（扮演 Stan）创建了这些文件，它们实际上还没有被传输到任何地方，您可以模拟成除创建者和发送者 Stan 之外的其他人。作为 Ruth，输入以下命令创建一个名为`exampleruthstore`的密钥库，并将证书导入到别名为`stan`的条目中。

```java
keytool -import -alias stan -file StanSmith.cer -keystore exampleruthstore

```

由于密钥库尚不存在，`keytool`将为您创建它。它会提示您输入密钥库密码。

`keytool`打印证书信息并要求您验证它；例如，通过将显示的证书指纹与从另一个（受信任的）信息源获得的指纹进行比较。（每个指纹是一个相对较短的数字，可以唯一且可靠地识别证书。）例如，在现实世界中，您可以打电话给 Stan 并询问他应该是什么指纹。他可以通过执行命令获取他创建的`StanSmith.cer`文件的指纹

```java
keytool -printcert -file StanSmith.cer

```

如果他看到的指纹与`keytool`向您报告的指纹相同，则您都可以假定证书在传输过程中未被修改。您可以放心地让`keytool`继续将一个“受信任的证书”条目放入您的密钥库中。该条目包含来自文件`StanSmith.cer`的公钥证书数据。`keytool`为这个新条目分配别名`stan`。
