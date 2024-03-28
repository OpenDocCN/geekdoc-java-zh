# 签署 JAR 文件

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/signing.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/signing.html)

你使用 JAR 签名和验证工具来签署 JAR 文件并为签名加上时间戳。你通过使用 `jarsigner` 命令来调用 JAR 签名和验证工具，因此我们简称为 "Jarsigner"。

要签署一个 JAR 文件，你首先必须拥有一个私钥。私钥及其关联的公钥证书存储在受密码保护的数据库中，称为 *keystores*。一个 keystore 可以保存许多潜在签署者的密钥。keystore 中的每个密钥可以通过一个 *alias* 来标识，通常是拥有该密钥的签署者的名称。例如，属于 Rita Jones 的密钥可能具有别名 "rita"。

签署 JAR 文件的命令的基本形式是

```java
jarsigner *jar-file alias*

```

在这个命令中：

+   `jar-file` 是要签署的 JAR 文件的路径名。

+   `alias` 是标识要用于签署 JAR 文件和密钥关联证书的私钥的别名。

Jarsigner 工具会提示你输入 keystore 和别名的密码。

这个命令的基本形式假定要使用的 keystore 存储在你的主目录中名为 `.keystore` 的文件中。它将创建名称分别为 `x.SF` 和 `x.DSA` 的签名和签名块文件，其中 `x` 是别名的前八个字母，全部转换为大写。这个基本命令将*覆盖*原始 JAR 文件，用已签名的 JAR 文件替换。

在实践中，你可能希望使用其中一个或多个可用的命令选项。例如，鼓励为签名加上时间戳，以便部署应用程序的任何工具都可以验证用于签署 JAR 文件的证书在签署文件时是有效的。如果未包含时间戳，则 Jarsigner 工具会发出警告。

选项位于`jar-file`路径名之前。以下表格描述了可用的选项：

**Jarsigner 命令选项**

| 选项 | 描述 |
| --- | --- |
| `-keystore` *url* | 指定要使用的 keystore，如果你不想使用默认的 `.keystore` 数据库。 |
| `-sigfile` *file* | 指定 .SF 和 .DSA 文件的基本名称，如果你不希望基本名称取自你的别名。 *file* 必须仅由大写字母（A-Z）、数字（0-9）、连字符（-）和下划线（_）组成。 |
| `-signedjar` *file* | 指定要生成的已签名 JAR 文件的名称，如果你不希望原始未签名文件被已签名文件覆盖。 |
| `-tsa` *url* | 使用由 URL 标识的时间戳机构（TSA）为签名生成时间戳。 |
| `-tsacert` *alias* | 使用由 *alias* 标识的 TSA 的公钥证书为签名生成时间戳。 |
| `-altsigner` *class* | 表示要使用另一种签名机制来时间戳签名。完全限定的类名标识所使用的类。 |
| `-altsignerpath` *classpathlist* | 提供`altsigner`选项标识的类的路径以及类所依赖的任何 JAR 文件。 |

## 示例

让我们看几个使用 Jarsigner 工具签署 JAR 文件的示例。在这些示例中，我们假设以下情况：

+   您的别名是"johndoe"。

+   您要使用的密钥库位于当前工作目录中名为"mykeys"的文件中。

+   您要用于时间戳签名的 TSA 位于`http://tsa.url.example.com`。

在这些假设下，您可以使用此命令对名为`app.jar`的 JAR 文件进行签名：

```java
jarsigner -keystore mykeys -tsa http://tsa.url.example.com app.jar johndoe

```

您将被提示输入密钥库和别名的密码。因为此命令不使用`-sigfile`选项，因此创建的.SF 和.DSA 文件将命名为`JOHNDOE.SF`和`JOHNDOE.DSA`。因为该命令不使用`-signedjar`选项，所以生成的签名文件将覆盖`app.jar`的原始版本。

让我们看看如果您使用不同的选项组合会发生什么：

```java
jarsigner -keystore mykeys -sigfile SIG -signedjar SignedApp.jar 
          -tsacert testalias app.jar johndoe

```

签名和签名块文件将分别命名为`SIG.SF`和`SIG.DSA`，已签名的 JAR 文件`SignedApp.jar`将放置在当前目录中。原始未签名的 JAR 文件将保持不变。此外，签名将使用 TSA 的公钥证书时间戳，标识为`testalias`。

## 附加信息

JAR 签名和验证工具的完整参考页面在线查看：[安全工具概述](https://docs.oracle.com/javase/8/docs/technotes/guides/security/SecurityToolsSummary.html)

* * *

**注意：**当证书是自签名时，应用程序的发布者将显示为`UNKNOWN`。有关更多信息，请参阅[从列为`UNKNOWN`的发布者运行应用程序是否安全？](http://www.java.com/en/download/faq/self_signed.xml)。

* * *
