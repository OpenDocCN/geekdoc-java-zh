# 关于 BreakIterator 类

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/about.html`](https://docs.oracle.com/javase/tutorial/i18n/text/about.html)

`BreakIterator`类是区域敏感的，因为文本边界随语言而变化。例如，换行的语法规则并非所有语言都相同。要确定`BreakIterator`类支持哪些区域设置，请调用`getAvailableLocales`方法，如下所示：

```java
Locale[] locales = BreakIterator.getAvailableLocales();

```

您可以使用`BreakIterator`类分析四种边界类型：字符、单词、句子和潜在的换行符。在实例化`BreakIterator`时，调用适当的工厂方法：

+   `getCharacterInstance`

+   `getWordInstance`

+   `getSentenceInstance`

+   `getLineInstance`

每个`BreakIterator`实例只能检测一种类型的边界。例如，如果您想定位字符和单词边界，您需要创建两个单独的实例。

`BreakIterator`具有一个想象的光标，指向文本字符串中的当前边界。您可以使用`previous`和`next`方法在文本中移动此光标。例如，如果您使用`getWordInstance`创建了一个`BreakIterator`，每次调用`next`方法时，光标都会移动到文本中的下一个单词边界。光标移动方法返回一个整数，指示边界的位置。此位置是文本字符串中将跟随边界的字符的索引。与字符串索引一样，边界是从零开始的。第一个边界在 0 处，最后一个边界是字符串的长度。以下图显示了`next`和`previous`方法在文本行中检测到的单词边界：

![带有边界指示的文本“希望是羽毛的东西”。](img/i18n-4.gif)

*此图已经缩小以适应页面。

点击图像以查看其自然大小。*

您应该仅将`BreakIterator`类与自然语言文本一起使用。要对编程语言进行标记化，请使用`StreamTokenizer`类。

接下来的部分为每种边界分析类型提供示例。编码示例来自名为`BreakIteratorDemo.java`的源代码文件。
