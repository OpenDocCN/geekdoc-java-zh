# 数字和货币

> 原文：[`docs.oracle.com/javase/tutorial/i18n/format/numberintro.html`](https://docs.oracle.com/javase/tutorial/i18n/format/numberintro.html)

程序以与语言环境无关的方式存储和操作数字。在显示或打印数字之前，程序必须将其转换为符合语言环境的`String`格式。例如，在法国，数字 123456.78 应格式化为 123 456,78，在德国应显示为 123.456,78。在本节中，您将学习如何使您的程序独立于语言环境的小数点、千位分隔符和其他格式化属性的约定。

## 使用预定义格式

使用`NumberFormat`类提供的工厂方法，您可以获得针对数字、货币和百分比的特定于语言环境的格式。

## 使用模式进行格式化

使用`DecimalFormat`类，您可以使用`String`模式指定数字的格式。`DecimalFormatSymbols`类允许您修改格式化符号，如小数分隔符和减号。
