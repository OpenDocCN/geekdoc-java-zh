# 设计考虑事项

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/design.html`](https://docs.oracle.com/javase/tutorial/i18n/text/design.html)

要编写能够无缝运行于任何语言和任何脚本的代码，有几点需要牢记。

| 考虑事项 | 原因 |
| --- | --- |
| 避免使用`char`数据类型的方法。 | 避免使用`char`原始数据类型或使用`char`数据类型的方法，因为使用该数据类型的代码对补充字符不起作用。对于需要`char`类型参数的方法，尽可能使用相应的`int`方法。例如，使用`Character.isDigit(int)`方法而不是`Character.isDigit(char)`方法。 |
| 使用`isValidCodePoint`方法验证代码点值。 | 代码点被定义为`int`数据类型，允许值超出从 0x0000 到 0x10FFFF 的有效代码点值范围。出于性能原因，接受代码点值作为参数的方法不会检查参数的有效性，但您可以使用`isValidCodePoint`方法检查该值。 |
| 使用`codePointCount`方法计算字符数。 | `String.length()`方法返回字符串中代码单元或 16 位`char`值的数量。如果字符串包含补充字符，则计数可能会误导，因为它不会反映真实的代码点数量。要准确计算字符数（包括补充字符），请使用`codePointCount`方法。 |
| 使用`String.toUpperCase(int codePoint)`和`String.toLowerCase(int codePoint)`方法而不是`Character.toUpperCase(int codePoint)`或`Character.toLowerCase(int codePoint)`方法。 | 虽然`Character.toUpperCase(int)`和`Character.toLowerCase(int)`方法可以处理代码点值，但有些字符无法进行一对一转换。例如，德语小写字符ß在转换为大写时变为两个字符 SS。同样，希腊语小写 Sigma 字符在字符串中的位置不同而有所不同。`Character.toUpperCase(int)`和`Character.toLowerCase(int)`方法无法处理这些情况；然而，`String.toUpperCase`和`String.toLowerCase`方法可以正确处理这些情况。 |
| 删除字符时要小心。 | 在调用`StringBuilder.deleteCharAt(int index)`或`StringBuffer.deleteCharAt(int index)`方法时，索引指向补充字符时，只会删除该字符的前半部分（第一个`char`值）。首先，调用`Character.charCount`方法对字符进行检查，以确定必须删除一个或两个`char`值。 |
| 在对序列中的字符进行反转时要小心。当在包含补充字符的文本上调用`StringBuffer.reverse()`或`StringBuilder.reverse()`方法时，高低代理对会被反转，导致不正确甚至可能无效的代理对。 |
