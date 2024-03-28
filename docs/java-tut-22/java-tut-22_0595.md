# 教训：格式化

> 原文：[`docs.oracle.com/javase/tutorial/i18n/format/index.html`](https://docs.oracle.com/javase/tutorial/i18n/format/index.html)

本课程解释了如何格式化数字、货币、日期、时间和文本消息。因为最终用户可以看到这些数据元素，它们的格式必须符合各种文化习俗。遵循本课程中的示例将教会您如何：

+   以区域设置敏感的方式格式化数据元素

+   保持您的代码与区域设置无关

+   避免为特定区域编写格式化程序的需要

## 数字和货币

本节解释了如何使用`NumberFormat`、`DecimalFormat`和`DecimalFormatSymbols`类。

## 日期和时间

* * *

**版本说明：** 本日期和时间部分使用了`java.util`包中的日期和时间 API。在 JDK 8 发布的`java.time`API 中，提供了一个全面的日期和时间模型，相比`java.util`类有显著的改进。`java.time`API 在日期时间教程中有详细描述。旧版日期时间代码页面可能会引起特别关注。

* * *

本节重点介绍了`DateFormat`、`SimpleDateFormat`和`DateFormatSymbols`类。

## 消息

本节展示了`MessageFormat`和`ChoiceFormat`类如何帮助您解决在格式化文本消息时可能遇到的一些问题。
