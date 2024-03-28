# 比较字符串

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/collationintro.html`](https://docs.oracle.com/javase/tutorial/i18n/text/collationintro.html)

应用程序对文本进行排序时会执行频繁的字符串比较。例如，报告生成器在对字符串列表按字母顺序排序时执行字符串比较。

如果您的应用程序受众仅限于讲英语的人，您可能可以使用`String.compareTo`方法执行字符串比较。`String.compareTo`方法对两个字符串中的 Unicode 字符执行二进制比较。然而，对于大多数语言，这种二进制比较不能被依赖来排序字符串，因为 Unicode 值不对应字符的相对顺序。

幸运的是，[`Collator`](https://docs.oracle.com/javase/8/docs/api/java/text/Collator.html)类允许您的应用程序为不同语言执行字符串比较。在本节中，您将学习如何在排序文本时使用`Collator`类。

## 执行与语言环境无关的比较

排序规则定义字符串的排序顺序。这些规则因语言环境而异，因为各种自然语言对单词的排序方式不同。使用`Collator`类提供的预定义排序规则，您可以以与语言环境无关的方式对字符串进行排序。

## 自定义排序规则

在某些情况下，`Collator`类提供的预定义排序规则可能不适用于您。例如，您可能希望对不受`Collator`支持的语言的字符串进行排序。在这种情况下，您可以定义自己的排序规则，并将其分配给`RuleBasedCollator`对象。

## 提高排序性能

使用`CollationKey`类，您可以提高字符串比较的效率。该类将`String`对象转换为遵循给定`Collator`规则的排序键。
