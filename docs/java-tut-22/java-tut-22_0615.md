# 字符和字符串 API

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/characterClass.html`](https://docs.oracle.com/javase/tutorial/i18n/text/characterClass.html)

`Character` 类封装了 `char` 数据类型。对于 J2SE 发布的第 5 版，许多方法被添加到 `Character` 类中以支持补充字符。这个 API 分为两类：将 `char` 和代码点值之间进行转换的方法以及验证或映射代码点的方法。

本节描述了 `Character` 类中可用方法的一个子集。有关可用 API 的完整列表，请参阅 [`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) 类规范。

## 转换方法和 Character 类

以下表格包括了 `Character` 类中最有用的转换方法或便于转换的方法。`codePointAt` 和 `codePointBefore` 方法包含在此列表中，因为文本通常以序列的形式出现，比如一个 `String`，这些方法可以用来提取所需的子字符串。

| 方法 | 描述 |
| --- | --- |
| [`toChars(int codePoint, char[] dst, int dstIndex)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#toChars-int-char:A-int-) [`toChars(int codePoint)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#toChars-int-) | 将指定的 Unicode 代码点转换为其 UTF-16 表示，并将其放入一个 `char` 数组中。示例用法：`Character.toChars(0x10400)` |
| [`toCodePoint(char high, char low)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#toCodePoint-char-char-) | 将指定的代理对转换为其补充代码点值。 |

| [`codePointAt(char[] a, int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointAt-char:A-int-) [`codePointAt(char[] a, int index, int limit)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointAt-char:A-int-int-)

[`codePointAt(CharSequence seq, int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointAt-java.lang.CharSequence-int-) | 返回指定索引处的 Unicode 代码点。第三个方法接受一个 `CharSequence`，第二个方法对索引施加了上限。 |

| [`codePointBefore(char[] a, int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointBefore-char:A-int-) [`codePointBefore(char[] a, int index, int start)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointBefore-char:A-int-int-)

[`codePointBefore(CharSequence seq, int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointBefore-java.lang.CharSequence-int-)

[`codePointBefore(char[], int, int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointBefore-char:A-int-int-) | 返回指定索引之前的 Unicode 代码点。第三个方法接受`CharSequence`，其他方法接受`char`数组。第二个方法对索引强制执行下限。 |

| [`charCount(int codePoint)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#charCount-int-) | 对于可以由单个`char`表示的字符，返回值为 1。对于需要两个`char`表示的补充字符，返回值为 2。 |
| --- | --- |

## 字符类中的验证和映射方法

以前使用`char`原始数据类型的一些方法，如`isLowerCase(char)`和`isDigit(char)`，已被支持补充字符的方法所取代，如`isLowerCase(int)`和`isDigit(int)`。以前的方法得到支持，但不适用于补充字符。为了创建一个全球应用程序并确保您的代码与任何语言无缝配合，建议您使用这些方法的新形式。

请注意，出于性能原因，大多数接受代码点的方法不会验证代码点参数的有效性。您可以使用`isValidCodePoint`方法进行验证。

以下表格列出了`Character`类中的一些验证和映射方法。

| 方法 | 描述 |
| --- | --- |
| [`isValidCodePoint(int codePoint)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isValidCodePoint-int-) | 如果代码点在 0x0000 到 0x10FFFF（包括）范围内，则返回 true。 |
| [`isSupplementaryCodePoint(int codePoint)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isSupplementaryCodePoint-int-) | 如果代码点在 0x10000 到 0x10FFFF（包括）范围内，则返回 true。 |
| [`isHighSurrogate(char)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isHighSurrogate-char-) | 如果指定的`char`在\uD800 到\uDBFF（包括）的高代理范围内，则返回 true。 |
| [`isLowSurrogate(char)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isLowSurrogate-char-) | 如果指定的`char`在\uDC00 到\uDFFF（包括）的低代理范围内，则返回 true。 |
| [`isSurrogatePair(char high, char low)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isSurrogatePair-char-char-) | 如果指定的高代理和低代理代码值表示有效的代理对，则返回 true。 |
| [`codePointCount(CharSequence, int, int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointCount-java.lang.CharSequence-int-int-) [`codePointCount(char[], int, int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#codePointCount-char:A-int-int-) | 返回`CharSequence`或`char`数组中的 Unicode 代码点数。 |
| [`isLowerCase(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isLowerCase-int-) [`isUpperCase(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isUpperCase-int-) | 如果指定的 Unicode 代码点是小写或大写字符，则返回 true。 |
| [`isDefined(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isDefined-int-) | 如果指定的 Unicode 代码点在 Unicode 标准中定义，则返回 true。 |
| [`isJavaIdentifierStart(char)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isJavaIdentifierStart-char-) [`isJavaIdentifierStart(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isJavaIdentifierStart-int-) | 如果指定的字符或 Unicode 代码点可作为 Java 标识符中的第一个字符，则返回 true。 |

| [`isLetter(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isLetter-int-) [`isDigit(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isDigit-int-)

[`isLetterOrDigit(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isLetterOrDigit-int-) | 如果指定的 Unicode 代码点是字母、数字或字母数字，则返回 true。 |

| [`getDirectionality(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#getDirectionality-int-) | 返回给定 Unicode 代码点的 Unicode 方向性属性。 |
| --- | --- |
| [`Character.UnicodeBlock.of(int codePoint)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.UnicodeBlock.html#of-int-) | 返回表示包含给定 Unicode 代码点的 Unicode 块的对象，如果代码点不是已定义块的成员，则返回`null`。 |

## 字符串类中的方法

`String`、`StringBuffer` 和 `StringBuilder` 类也有构造函数和方法，可以处理补充字符。以下表格列出了一些常用方法。要查看可用 API 的完整列表，请参阅 [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)、[`StringBuffer`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html) 和 [`StringBuilder`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html) 类的 javadoc。

| 构造函数或方法 | 描述 |
| --- | --- |
| [`String(int[] codePoints, int offset, int count)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#String-int:A-int-int-) | 分配一个新的 `String` 实例，其中包含来自 Unicode 代码点数组的子数组的字符。 |

| [`String.codePointAt(int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#codePointAt-int-) [`StringBuffer.codePointAt(int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html#codePointAt-int-)

[`StringBuilder.codePointAt(int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#codePointAt-int-) | 返回指定索引处的 Unicode 代码点。 |

| [`String.codePointBefore(int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#codePointBefore-int-) [`StringBuffer.codePointBefore(int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html#codePointBefore-int-)

[`StringBuilder.codePointBefore(int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#codePointBefore-int-) | 返回指定索引之前的 Unicode 代码点。 |

| [`String.codePointCount(int beginIndex, int endIndex)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#codePointCount-int-int-) [`StringBuffer.codePointCount(int beginIndex, int endIndex)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html#codePointCount-int-int-)

[`StringBuilder.codePointCount(int beginIndex, int endIndex)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#codePointCount-int-int-) | 返回指定范围内的 Unicode 代码点数。 |

| [`StringBuffer.appendCodePoint(int codePoint)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html#appendCodePoint-int-) [`StringBuilder.appendCodePoint(int codePoint)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#appendCodePoint-int-) | 将指定代码点的字符串表示附加到序列。 |
| --- | --- |

| [`String.offsetByCodePoints(int index, int codePointOffset)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#offsetByCodePoints-int-int-) [`StringBuffer.offsetByCodePoints(int index, int codePointOffset)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html#offsetByCodePoints-int-int-)

[`StringBuilder.offsetByCodePoints(int index, int codePointOffset)`](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#offsetByCodePoints-int-int-) | 返回给定索引偏移给定代码点数量后的索引。 |
