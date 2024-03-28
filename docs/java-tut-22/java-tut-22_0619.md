# 检测文本边界

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/boundaryintro.html`](https://docs.oracle.com/javase/tutorial/i18n/text/boundaryintro.html)

操纵文本的应用程序需要定位文本内的边界。例如，考虑一些文字处理器的常见功能：突出显示一个字符，剪切一个单词，将光标移动到下一个句子，以及在行尾换行一个单词。为了执行这些功能，文字处理器必须能够检测文本中的逻辑边界。幸运的是，您不必编写自己的例程来执行边界分析。相反，您可以利用[`BreakIterator`](https://docs.oracle.com/javase/8/docs/api/java/text/BreakIterator.html)类提供的方法。

## 关于 BreakIterator 类

本节讨论了`BreakIterator`类的实例化方法和虚拟光标。

## 字符边界

在本节中，您将了解用户字符和 Unicode 字符之间的区别，以及如何使用`BreakIterator`定位用户字符。

## 词边界

如果您的应用程序需要在文本中选择或定位单词，使用`BreakIterator`会很有帮助。

## 句子边界

确定句子边界可能会有问题，因为许多书面语言中句子终止符的使用是模棱两可的。本节将讨论您可能遇到的一些问题，以及`BreakIterator`如何处理这些问题。

## 行边界

本节描述了如何使用`BreakIterator`在文本字符串中定位潜在的换行符。
