# 检查表

> 原文：[`docs.oracle.com/javase/tutorial/i18n/intro/checklist.html`](https://docs.oracle.com/javase/tutorial/i18n/intro/checklist.html)

许多程序在最初编写时并未国际化。这些程序可能最初是原型，或者可能并非用于国际分发。如果你必须国际化现有程序，请执行以下步骤：

## 识别文化相关数据

文本消息是随文化变化最明显的数据形式。然而，其他类型的数据可能会随地区或语言而变化。以下列表包含了文化相关数据的例子：

+   消息

+   GUI 组件上的标签

+   在线帮助

+   声音

+   颜色

+   图形

+   图标

+   日期

+   时间

+   数字

+   货币

+   测量

+   电话号码

+   敬语和个人头衔

+   邮政地址

+   页面布局

## 在资源包中隔离可翻译文本

翻译是昂贵的。你可以通过将必须翻译的文本隔离在`ResourceBundle`对象中来帮助降低成本。可翻译的文本包括状态消息、错误消息、日志文件条目和 GUI 组件标签。这些文本包含在尚未国际化的程序中。你需要找到所有显示给最终用户的包含文本的出现。例如，你应该清理这样的代码：

```java
String buttonLabel = "OK";
// ...
JButton okButton = new JButton(buttonLabel);

```

详细信息请参见隔离特定区域数据部分。

## 处理复合消息

复合消息包含可变数据。在消息"The disk contains 1100 files."中，整数 1100 可能会变化。这个消息很难翻译，因为整数在句子中的位置在所有语言中都不相同。下面的消息是不可翻译的，因为句子元素的顺序是由串联固定的：

```java
Integer fileCount;
// ...
String diskStatus = "The disk contains " + fileCount.toString() + " files";

```

尽可能避免构建复合消息，因为它们很难翻译。然而，如果你的应用程序需要复合消息，你可以使用消息部分中描述的技术来处理它们。

## 格式化数字和货币

如果你的应用程序显示数字和货币，你必须以与地区无关的方式格式化它们。以下代码尚未国际化，因为它在所有国家中都不会正确显示数字：

```java
Double amount;
TextField amountField;
// ...
String displayAmount = amount.toString();
amountField.setText(displayAmount);

```

你应该用一个能正确格式化数字的例程替换前面的代码。Java 编程语言提供了几个格式化数字和货币的类。这些类在数字和货币部分有讨论。

## 格式化日期和时间

日期和时间格式因地区和语言而异。如果你的代码包含如下语句，你需要进行更改：

```java
Date currentDate = new Date();
TextField dateField;
// ...
String dateString = currentDate.toString();
dateField.setText(dateString);

```

如果你使用日期格式化类，你的应用程序可以在全球范围内正确显示日期和时间。有关示例和说明，请参见日期和时间部分。

## 使用 Unicode 字符属性

以下代码尝试验证一个字符是否为字母：

```java
char ch;
// This code is incorrect
if ((ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z'))

```

注意类似这样的代码，因为它在除英语以外的语言中无法正常工作。例如，`if`语句在德语单词 Grün 中漏掉了字符ü。

`Character`比较方法使用 Unicode 标准来识别字符属性。因此，您应该用以下代码替换先前的代码：

```java
char ch;
// ...
if (Character.isLetter(ch))

```

有关`Character`比较方法的更多信息，请参阅检查字符属性部分。

## 正确比较字符串

在对文本进行排序时，通常需要比较字符串。如果文本是显示的，您不应该使用`String`类的比较方法。一个未国际化的程序可能会这样比较字符串：

```java
String target;
String candidate;
// ...
if (target.equals(candidate)) {
// ...
if (target.compareTo(candidate) < 0) {
// ...

```

`String.equals`和`String.compareTo`方法执行二进制比较，在大多数语言中排序时效率低下。相反，您应该使用`Collator`类，该类在比较字符串部分有描述。

## 转换非 Unicode 文本

Java 编程语言中的字符是以 Unicode 编码的。如果您的应用程序处理非 Unicode 文本，您可能需要将其转换为 Unicode。有关更多信息，请参阅转换非 Unicode 文本部分。
