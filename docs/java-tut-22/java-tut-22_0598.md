# 自定义格式

> 原文：[`docs.oracle.com/javase/tutorial/i18n/format/decimalFormat.html`](https://docs.oracle.com/javase/tutorial/i18n/format/decimalFormat.html)

你可以使用`DecimalFormat`类将十进制数格式化为特定于区域设置的字符串。这个类允许你控制前导和尾随零的显示，前缀和后缀，分组（千位）分隔符和小数分隔符。如果你想要更改格式化符号，比如小数分隔符，你可以与`DecimalFormat`类一起使用`DecimalFormatSymbols`。这些类在数字格式化方面提供了很大的灵活性，但可能会使你的代码变得更加复杂。

以下文本使用示例演示了`DecimalFormat`和`DecimalFormatSymbols`类。本材料中的代码示例来自一个名为`DecimalFormatDemo`的示例程序。

## 构建模式

你可以通过模式`String`指定`DecimalFormat`的格式属性。模式决定了格式化后的数字是什么样子的。有关模式语法的完整描述，请参见数字格式模式语法。

接下来的示例通过将模式`String`传递给`DecimalFormat`构造函数来创建一个格式化器。`format`方法接受一个`double`值作为参数，并以`String`形式返回格式化后的数字：

```java
DecimalFormat myFormatter = new DecimalFormat(pattern);
String output = myFormatter.format(value);
System.out.println(value + " " + pattern + " " + output);

```

前面代码的输出描述在以下表中。`value`是要格式化的数字，一个`double`，`pattern`是指定格式属性的`String`，`output`是一个`String`，表示格式化后的数字。

`DecimalFormatDemo`程序的输出

| `value` | `pattern` | `output` | 解释 |
| --- | --- | --- | --- |
| 123456.789 | ###,###.### | 123,456.789 | 井号（#）表示一个数字，逗号是分组分隔符的占位符，句点是小数分隔符的占位符。 |
| 123456.789 | ###.## | 123456.79 | `value`小数点右侧有三位数字，但`pattern`只有两位。`format`方法通过四舍五入处理这个问题。 |
| 123.78 | 000000.000 | 000123.780 | `pattern`指定了前导和尾随零，因为使用 0 字符而不是井号（#）。 |
| 12345.67 | $###,###.### | $12,345.67 | `pattern`中的第一个字符是美元符号($)。请注意，它紧跟在格式化的`output`中最左边的数字之前。 |
| 12345.67 | \u00A5###,###.### | ¥12,345.67 | `pattern`指定了日元（¥）的货币符号，Unicode 值为 00A5。 |

## 区域敏感格式化

前面的示例创建了一个默认`Locale`的`DecimalFormat`对象。如果你想要一个非默认`Locale`的`DecimalFormat`对象，你可以实例化一个`NumberFormat`，然后将其转换为`DecimalFormat`。以下是一个示例：

```java
NumberFormat nf = NumberFormat.getNumberInstance(loc);
DecimalFormat df = (DecimalFormat)nf;
df.applyPattern(pattern);
String output = df.format(value);
System.out.println(pattern + " " + output + " " + loc.toString());

```

运行上一个代码示例会产生以下输出。格式化的数字，位于第二列，随`Locale`而变化：

```java
###,###.###      123,456.789     en_US
###,###.###      123.456,789     de_DE
###,###.###      123 456,789     fr_FR

```

到目前为止，这里讨论的格式化模式遵循美国英语的惯例。例如，在模式###,###.##中，逗号是千位分隔符，句点代表小数点。这种惯例是可以接受的，前提是您的最终用户不会接触到它。然而，一些应用程序，如电子表格和报表生成器，允许最终用户定义自己的格式化模式。对于这些应用程序，最终用户指定的格式化模式应使用本地化符号。在这些情况下，您需要在`DecimalFormat`对象上调用`applyLocalizedPattern`方法。

## 更改格式化符号

您可以使用[DecimalFormatSymbols](https://docs.oracle.com/javase/8/docs/api/java/text/DecimalFormatSymbols.html)类来更改`format`方法生成的格式化数字中出现的符号。这些符号包括小数分隔符、分组分隔符、减号和百分号等。

下一个示例演示了`DecimalFormatSymbols`类，通过对数字应用奇怪的格式来实现。这种不寻常的格式是通过调用`setDecimalSeparator`、`setGroupingSeparator`和`setGroupingSize`方法得到的。

```java
DecimalFormatSymbols unusualSymbols = new DecimalFormatSymbols(currentLocale);
unusualSymbols.setDecimalSeparator('|');
unusualSymbols.setGroupingSeparator('^');

String strange = "#,##0.###";
DecimalFormat weirdFormatter = new DecimalFormat(strange, unusualSymbols);
weirdFormatter.setGroupingSize(4);

String bizarre = weirdFormatter.format(12345.678);
System.out.println(bizarre);

```

运行时，此示例会以奇怪的格式打印数字：

```java
1²³⁴⁵|678

```

## 数字格式模式语法

您可以按照以下 BNF 图表指定的规则设计自己的数字格式模式：

```java
pattern    := subpattern{;subpattern}
subpattern := {prefix}integer{.fraction}{suffix}
prefix     := '\\u0000'..'\\uFFFD' - specialCharacters
suffix     := '\\u0000'..'\\uFFFD' - specialCharacters
integer    := '#'* '0'* '0'
fraction   := '0'* '#'*

```

在前面的图表中使用的符号在以下表格中有解释：

| 符号 | 描述 |
| --- | --- |
| `X*` | X 的 0 或多个实例 |
| `(X &#124; Y)` | X 或 Y 中的任意一个 |
| `X..Y` | 从 X 到 Y 的任意字符，包括 X 和 Y |
| `S - T` | S 中的字符，但不包括 T 中的字符 |
| `{X}` | X 是可选的 |

在前面的 BNF 图表中，第一个子模式指定了正数的格式。第二个子模式是可选的，指定了负数的格式。

尽管在 BNF 图表中没有说明，但逗号可能出现在整数部分内。

在子模式中，您可以使用特殊符号指定格式。这些符号在以下表格中描述：

| 符号 | 描述 |
| --- | --- |
| 0 | 一个数字 |
| # | 一个数字，零表示不存在 |
| . | 小数分隔符的占位符 |
| , | 用于分组分隔符的占位符 |
| E | 用于指数格式中的尾数和指数的分隔符 |
| ; | 分隔格式 |
| - | 默认负数前缀 |
| % | 乘以 100 并显示为百分比 |
| ? | 乘以 1000 并显示为千分数 |
| ¤ | 货币符号；替换为货币符号；如果双倍，则替换为国际货币符号；如果在模式中存在，则使用货币小数分隔符而不是小数分隔符 |
| X | 前缀或后缀中可以使用任何其他字符 |
| ' | 用于引用前缀或后缀中的特殊字符 |
