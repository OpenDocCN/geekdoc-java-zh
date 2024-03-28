# 国际化域名

> 原文：[`docs.oracle.com/javase/tutorial/i18n/network/idn.html`](https://docs.oracle.com/javase/tutorial/i18n/network/idn.html)

从历史上看，互联网域名仅包含 ASCII 符号。随着互联网的普及和在全球范围内的采用，支持域名的国际化变得必要，特别是支持包含 Unicode 字符的域名。

**应用程序中的国际化域名**（IDNA）机制被采用为将 Unicode 字符转换为标准 ASCII 域名的标准，从而保持域名系统的稳定性。该系统执行查找服务，将用户友好的名称转换为网络地址。

国际化域名的示例：

+   `http://清华大学.cn`

+   `http://www.транспорт.com`

如果您点击这些链接，您会看到地址栏中表示的 Unicode 域名被替换为 ASCII 字符串。

要在应用程序中实现类似的功能，[`java.net.IDN`](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html)类提供了方法，用于在 ASCII 和非 ASCII 格式之间转换域名。

| 方法 | 目的 |
| --- | --- |
| [`toASCII(String)`](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html#toASCII-java.lang.String-) [`toASCII(String, flag)`](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html#toASCII-java.lang.String-int-) | 在将 IDN 发送到域名解析系统或写入期望 ASCII 字符的文件（例如 DNS 主文件）之前使用。如果输入字符串不符合[RFC 3490](http://www.ietf.org/rfc/rfc3490.txt)，这些方法会抛出`IllegalArgumentException`异常。 |
| [`toUnicode(String)`](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html#toUnicode-java.lang.String-) [`toUnicode(String, flag)`](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html#toUnicode-java.lang.String-int-) | 在向用户显示名称时使用，例如从 DNS 区域获取的名称。此方法将字符串从 ASCII 兼容编码（ACE）转换为 Unicode 代码点。此方法永远不会失败；在出现错误时，输入字符串保持不变并原样返回。 |

可选的`flag`参数指定转换过程的行为。`ALLOW_UNASSIGNED`标志允许包含 Unicode 3.2 中未分配的代码点。`USE_STD3_ASCII_RULES`标志确保遵守 STD-3 ASCII 规则。您可以单独使用这些标志或逻辑上合并它们。如果不需要任何标志，请使用该方法的单参数版本。
