# 转换非 Unicode 文本

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/convertintro.html`](https://docs.oracle.com/javase/tutorial/i18n/text/convertintro.html)

在 Java 编程语言中，`char`值表示 Unicode 字符。Unicode 是一种支持世界主要语言的 16 位字符编码。您可以在[Unicode 联盟网站](http://www.unicode.org/)了解有关 Unicode 标准的更多信息。

目前很少有文本编辑器支持 Unicode 文本输入。我们用来编写本节代码示例的文本编辑器仅支持 ASCII 字符，这些字符仅限于 7 位。为了表示 ASCII 无法表示的 Unicode 字符，例如ö，我们使用`\uXXXX`转义序列。转义序列中的每个`X`都是一个十六进制数字。以下示例显示了如何使用转义序列表示ö字符：

```java
String str = "\u00F6";
char c = '\u00F6';
Character letter = new Character('\u00F6');

```

世界各地的系统使用各种字符编码。目前，很少有这些编码符合 Unicode 标准。因为您的程序期望字符为 Unicode 格式，所以从系统获取的文本数据必须转换为 Unicode 格式，反之亦然。当文本文件的编码与 Java 虚拟机的默认文件编码匹配时，文本数据会自动转换为 Unicode 格式。您可以通过创建一个`OutputStreamWriter`并询问其规范名称来确定默认文件编码：

```java
OutputStreamWriter out = new OutputStreamWriter(new ByteArrayOutputStream());
System.out.println(out.getEncoding());

```

如果默认文件编码与您要处理的文本数据的编码不同，则必须自行进行转换。当处理来自其他国家或计算平台的文本时，您可能需要这样做。

本节讨论了用于将非 Unicode 文本转换为 Unicode 的 API。在使用这些 API 之前，您应该验证要转换为 Unicode 的字符编码是否受支持。支持的字符编码列表不是 Java 编程语言规范的一部分。因此，API 支持的字符编码可能因平台而异。要查看 Java 开发工具包支持的编码，请参阅[支持的编码](https://docs.oracle.com/javase/8/docs/technotes/guides/intl/encoding.doc.html)文档。

接下来的内容描述了将非 Unicode 文本转换为 Unicode 的两种技术。您可以将非 Unicode 字节数组转换为`String`对象，反之亦然。或者您可以在 Unicode 字符流和非 Unicode 文本的字节流之间进行转换。

## 字节编码和字符串

本节向您展示如何将非 Unicode 字节数组转换为`String`对象，反之亦然。

## 字符流和字节流

在本节中，您将学习如何在 Unicode 字符流和非 Unicode 文本的字节流之间进行转换。
