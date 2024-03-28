# 将证书导入为受信任的证书

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/rstep2.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/rstep2.html)

在授予已签署代码读取指定文件的权限之前，您需要将 Susan 的证书作为受信任的证书导入到您的密钥库中。

假设您已经从 Susan 那里收到了

+   已签署的 JAR 文件`sCount.jar`，其中包含`Count.class`文件，以及

+   包含与用于签署 JAR 文件的私钥对应的公钥证书的`Example.cer`文件。

即使您创建了这些文件并且它们实际上还没有被传输到任何地方，您也可以模拟成不是创建者和发送者 Susan 的其他人。假装您现在是 Ray。作为 Ray，您将创建一个名为`exampleraystore`的密钥库，并将其用于将证书导入到具有别名`susan`的条目中。

每当您使用指定尚不存在的密钥库的`keytool`命令时，都会创建一个密钥库。因此，我们可以通过单个`keytool`命令创建`exampleraystore`并导入证书。在命令窗口中执行以下操作。

1.  转到包含公钥证书文件`Example.cer`的目录。（实际上，您应该已经在那里，因为本课程假设您在整个过程中都保持在一个目录中。）

1.  在一行上输入以下命令：

    ```java
    keytool -import -alias susan
       -file Example.cer -keystore exampleraystore

    ```

由于密钥库尚不存在，将会创建它，并提示您输入密钥库密码；输入任何您想要的密码。

`keytool`命令将打印出证书信息，并要求您验证它，例如，通过将显示的证书指纹与从另一个（受信任的）信息源获得的指纹进行比较。 （每个指纹是一个相对较短的数字，可以唯一且可靠地识别证书。）例如，在现实世界中，您可能会打电话给 Susan，并询问她应该是什么指纹。她可以通过执行以下命令获取她创建的`Example.cer`文件的指纹

```java
keytool -printcert -file Example.cer

```

如果她看到的指纹与`keytool`向您报告的指纹相同，则证书在传输过程中未被修改。在这种情况下，您可以让`keytool`继续将受信任的证书条目放入密钥库中。该条目包含来自文件`Example.cer`的公钥证书数据，并被分配别名`susan`。
