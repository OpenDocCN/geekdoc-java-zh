# 课程：正则表达式

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/index.html`](https://docs.oracle.com/javase/tutorial/essential/regex/index.html)

本课程解释了如何使用`[`java.util.regex`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/package-summary.html)` API 进行正则表达式的模式匹配。尽管此包接受的语法类似于[Perl](http://www.perl.com)编程语言，但并不需要了解 Perl。本课程从基础知识开始，逐渐深入，涵盖更高级的技术。

介绍

提供了正则表达式的一般概述。还介绍了构成此 API 的核心类。

测试工具

定义了一个简单的应用程序，用于测试正则表达式的模式匹配。

字符串字面值

介绍了基本的模式匹配、元字符和引用。

字符类

描述了简单字符类、否定、范围、并集、交集和差集。

预定义字符类

描述了用于空白、单词和数字字符的基本预定义字符类。

量词

解释了贪婪、懒惰和占有量词，用于匹配指定表达式*x*的次数。

捕获组

解释了如何将多个字符视为单个单元。

边界匹配器

描述了行、单词和输入边界。

Pattern 类的方法

探讨了`Pattern`类的其他有用方法，并探索了高级功能，如使用标志进行编译和使用嵌入式标志表达式。

Matcher 类的方法

描述了`Matcher`类的常用方法。

PatternSyntaxException 类的方法

描述了如何检查`PatternSyntaxException`。

其他资源

要了解更多关于正则表达式的信息，请查阅此部分的其他资源。
