# 检查字符属性

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/charintro.html`](https://docs.oracle.com/javase/tutorial/i18n/text/charintro.html)

您可以根据字符的属性对字符进行分类。例如，X 是大写字母，4 是十进制数字。检查字符属性是验证最终用户输入的数据的常见方法。例如，如果您在网上销售书籍，订单输入屏幕应验证数量字段中的字符是否都是数字。

不习惯编写全球软件的开发人员可能会通过将其与字符常量进行比较来确定字符的属性。例如，他们可能会编写如下代码：

```java
char ch;
//...

// This code is WRONG!

// check if ch is a letter
if ((ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z'))
    // ...

// check if ch is a digit
if (ch >= '0' && ch <= '9')
    // ...

// check if ch is a whitespace
if ((ch == ' ') || (ch =='\n') || (ch == '\t'))
    // ...

```

前面的代码是*错误*的，因为它只适用于英语和少数其他语言。要使前面的示例国际化，将其替换为以下语句：

```java
char ch;
// ...

// This code is OK!

if (Character.isLetter(ch))
    // ...

if (Character.isDigit(ch))
    // ...

if (Character.isSpaceChar(ch))
    // ...

```

[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html)方法依赖于 Unicode 标准来确定字符的属性。Unicode 是一种支持世界主要语言的 16 位字符编码。在 Java 编程语言中，`char`值表示 Unicode 字符。如果您使用适当的`Character`方法检查`char`的属性，则您的代码将适用于所有主要语言。例如，如果字符是中文、德文、阿拉伯文或其他语言中的字母，则`Character.isLetter`方法返回`true`。

以下列表列出了一些最有用的`Character`比较方法。`Character` API 文档完全指定了这些方法。

+   `isDigit`

+   `isLetter`

+   `isLetterOrDigit`

+   `isLowerCase`

+   `isUpperCase`

+   `isSpaceChar`

+   `isDefined`

`Character.getType`方法返回字符的 Unicode 类别。每个类别对应于`Character`类中定义的常量。例如，对于字符 A，`getType`返回`Character.UPPERCASE_LETTER`常量。有关`getType`返回的类别常量的完整列表，请参阅[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) API 文档。以下示例显示了如何使用`getType`和`Character`类别常量。这些`if`语句中的所有表达式都为`true`：

```java
if (Character.getType('a') == Character.LOWERCASE_LETTER)
    // ...

if (Character.getType('R') == Character.UPPERCASE_LETTER)
    // ...

if (Character.getType('>') == Character.MATH_SYMBOL)
    // ...

if (Character.getType('_') == Character.CONNECTOR_PUNCTUATION)
    // ...

```
