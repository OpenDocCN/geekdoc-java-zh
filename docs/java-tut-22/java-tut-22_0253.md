# Unicode 支持

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/unicode.html`](https://docs.oracle.com/javase/tutorial/essential/regex/unicode.html)

从 JDK 7 版本开始，正则表达式模式匹配已扩展功能以支持 Unicode 6.0。

+   匹配特定代码点

+   Unicode 字符属性

## 匹配特定代码点

您可以使用形式为`\uFFFF`的转义序列匹配特定的 Unicode 代码点，其中`FFFF`是您想匹配的代码点的十六进制值。例如，`\u6771`匹配东方的汉字。

或者，您可以使用 Perl 风格的十六进制表示法指定一个代码点，`\x{...}`。例如：

```java
String hexPattern = "\x{" + Integer.toHexString(codePoint) + "}";

```

## Unicode 字符属性

每个 Unicode 字符除了其值之外，还具有某些属性或特性。您可以使用表达式`\p{*prop*}`匹配属于特定类别的单个字符。您可以使用表达式`\P{*prop*}`匹配*不*属于特定类别的单个字符。

支持的属性类型有脚本、区块和“通用”类别。

### 脚本

要确定代码点是否属于特定脚本，您可以使用`script`关键字或`sc`的简写形式，例如，`\p{script=Hiragana}`。或者，您可以在脚本名称前加上字符串`Is`，例如`\p{IsHiragana}`。

`Pattern`支持的有效脚本名称是[`UnicodeScript.forName`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.UnicodeScript.html#forName-java.lang.String-)接受的那些。

### 区块

可以使用`block`关键字或`blk`的简写形式指定一个区块，例如，`\p{block=Mongolian}`。或者，您可以在区块名称前加上字符串`In`，例如`\p{InMongolian}`。

`Pattern`支持的有效区块名称是[`UnicodeBlock.forName`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.UnicodeBlock.html#forName-java.lang.String-)接受的那些。

### 通用类别

类别可以用可选前缀`Is`指定。例如，`IsL`匹配 Unicode 字母的类别。类别也可以通过使用`general_category`关键字或简写形式`gc`来指定。例如，大写字母可以使用`general_category=Lu`或`gc=Lu`来匹配。

支持的类别是由[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html)类指定的[Unicode 标准](http://www.unicode.org/unicode/standard/standard.html)版本中的类别。
