# 规范化文本

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/normalizerapi.html`](https://docs.oracle.com/javase/tutorial/i18n/text/normalizerapi.html)

*规范化*是一种过程，通过该过程您可以对文本执行某些转换，使其在以前可能无法协调的方式中协调。比如说，您想要搜索或排序文本，在这种情况下，您需要将该文本规范化以考虑应表示为相同文本的代码点。

什么可以被规范化？当您需要转换带有变音符号的字符，更改所有字母大小写，分解连字，或将半角片假名字符转换为全角字符等时，规范化是适用的。

根据[Unicode 标准附录＃15](http://www.unicode.org/reports/tr15/)，规范化器的 API 支持[`java.text.Normalizer.Form`](https://docs.oracle.com/javase/8/docs/api/java/text/Normalizer.Form.html)中定义的以下四种 Unicode 文本规范化形式：

+   规范化形式 D（NFD）：规范分解

+   规范化形式 C（NFC）：规范分解，然后规范组合

+   规范化形式 KD（NFKD）：兼容性分解

+   规范化形式 KC（NFKC）：兼容性分解，然后规范组合

让我们看看拉丁小写字母"o"带分音符号如何通过使用这些规范化形式进行规范化：

| 原始单词 | NFC | NFD | NFKC | NFKD |
| --- | --- | --- | --- | --- |
| "schön" | "schön" | "scho\u0308n" | "schön" | "scho\u0308n" |

您可以注意到在 NFC 和 NFKC 中原始单词保持不变。这是因为在 NFD 和 NFKD 中，复合字符被映射到它们的规范分解形式。但是在 NFC 和 NFKC 中，如果可能的话，组合字符序列被映射到复合字符。对于分音符号，没有复合字符，因此在 NFC 和 NFKC 中保持分解状态。

在稍后表示的代码示例`NormSample.java`中，您还可以注意到另一个规范化特性。半角和全角片假名字符将具有相同的兼容性分解，因此它们是兼容性等效的。但是，它们不是规范等效的。

要确保您确实需要规范化文本，您可以使用`isNormalized`方法来确定给定的字符序列是否已规范化。如果此方法返回 false，则意味着您需要规范化此序列，并且应使用`normalize`方法，该方法根据指定的规范化形式规范化`char`值。例如，要将文本转换为规范分解形式，您将需要使用以下`normalize`方法：

```java
normalized_string = Normalizer.normalize(target_chars, Normalizer.Form.NFD);

```

此外，规范化方法会将重音符号重新排列为正确的规范顺序，因此您无需担心自行重新排列重音符号。

以下示例表示一个应用程序，使您能够选择规范化形式和模板进行规范化：

<applet code="NormSample" archive="examples/lib/NormSample.jar" width="550" height="230" alt="NormSample applet"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到 applet 运行，请安装至少 [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

这个 applet 的完整代码在 `NormSample.java` 中。
