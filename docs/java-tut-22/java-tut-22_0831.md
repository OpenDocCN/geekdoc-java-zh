# Code Signer 的步骤

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/signer.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/signer.html)

Code Signer 执行以下步骤：

1.  下载并尝试示例应用程序。

1.  创建包含类文件的 JAR 文件，使用`jar`工具。

1.  生成密钥（如果尚不存在），使用`keytool`的`-genkey`命令。

    * * *

    *可选步骤* 为公钥证书生成证书签名请求（CSR），并导入认证机构（CA）的响应。为简单起见（并且因为你只是假装是苏珊·琼斯），这一步被省略了。有关更多信息，请参阅为公钥证书生成证书签名请求（CSR）。

    * * *

1.  对 JAR 文件进行签名，使用`jarsigner`工具和私钥。

1.  导出公钥证书，使用`keytool`的`-export`命令。然后将签名的 JAR 文件和证书提供给接收者雷。
