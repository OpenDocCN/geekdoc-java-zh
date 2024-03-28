# 介绍

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/intro.html`](https://docs.oracle.com/javase/tutorial/essential/regex/intro.html)

## 什么是正则表达式？

*正则表达式*是一种描述一组字符串的方式，基于集合中每个字符串共享的共同特征。它们可用于搜索、编辑或操作文本和数据。您必须学习一种特定的语法来创建正则表达式，这种语法超出了 Java 编程语言的正常语法。正则表达式的复杂程度各不相同，但一旦您了解了它们构建的基础知识，就能够解读（或创建）任何正则表达式。

这个教程介绍了由[`java.util.regex`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/package-summary.html) API 支持的正则表达式语法，并提供了几个实际示例来说明各种对象是如何交互的。在正则表达式的世界中，有许多不同的风格可供选择，如 grep、Perl、Tcl、Python、PHP 和 awk。`java.util.regex` API 中的正则表达式语法与 Perl 中的最为相似。

## 此包中如何表示正则表达式？

`java.util.regex`包主要由三个类组成：[`Pattern`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)、[`Matcher`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html)和[`PatternSyntaxException`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/PatternSyntaxException.html)。

+   一个`Pattern`对象是正则表达式的编译表示。`Pattern`类不提供公共构造函数。要创建一个模式，您必须首先调用其`public static compile`方法之一，然后该方法将返回一个`Pattern`对象。这些方法接受一个正则表达式作为第一个参数；这个教程的前几课将教会您所需的语法。

+   一个`Matcher`对象是解释模式并对输入字符串执行匹配操作的引擎。与`Pattern`类一样，`Matcher`不定义公共构造函数。您可以通过在`Pattern`对象上调用`matcher`方法来获取`Matcher`对象。

+   一个`PatternSyntaxException`对象是指示正则表达式模式中语法错误的未经检查的异常。

这个教程的最后几课将详细探讨每个类。但首先，您必须了解正则表达式是如何构建的。因此，下一节介绍了一个简单的测试工具，将被反复用来探索它们的语法。
