# 自定义格式

> 原文：[`docs.oracle.com/javase/tutorial/i18n/format/simpleDateFormat.html`](https://docs.oracle.com/javase/tutorial/i18n/format/simpleDateFormat.html)

* * *

**版本说明：** 此日期和时间部分使用`java.util`包中的日期和时间 API。在 JDK 8 发布的`java.time`API 提供了一个全面的日期和时间模型，相比`java.util`类有显著改进。`java.time`API 在日期时间教程中有详细描述。旧版日期时间代码页面可能会引起特别关注。

* * *

前一节，使用预定义格式，描述了`DateFormat`类提供的格式样式。在大多数情况下，这些预定义格式是足够的。但是，如果你想创建自定义格式，可以使用[`SimpleDateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)类。

接下来的代码示例演示了`SimpleDateFormat`类的方法。你可以在名为`SimpleDateFormatDemo`的文件中找到示例的完整源代码。

## 关于模式

当你创建一个`SimpleDateFormat`对象时，你指定一个模式`String`。模式`String`的内容决定了日期和时间的格式。有关模式语法的完整描述，请参见日期格式模式语法中的表格。

以下代码根据传递给`SimpleDateFormat`构造函数的模式`String`格式化日期和时间。`format`方法返回的`String`包含要显示的格式化日期和时间。

```java
Date today;
String output;
SimpleDateFormat formatter;

formatter = new SimpleDateFormat(pattern, currentLocale);
today = new Date();
output = formatter.format(today);
System.out.println(pattern + " " + output);

```

以下表格显示了在指定美国`Locale`时前面代码示例生成的输出：

自定义日期和时间格式

| 模式 | 输出 |
| --- | --- |
| dd.MM.yy | 30.06.09 |
| yyyy.MM.dd G 'at' hh:mm:ss z | 2009.06.30 公元 at 08:29:36 PDT |
| EEE, MMM d, ''yy | 周二, 六月 30, '09 |
| h:mm a | 8:29 PM |
| H:mm | 8:29 |
| H:mm:ss:SSS | 8:28:36:249 |
| K:mm a,z | 8:29 AM,PDT |
| yyyy.MMMMM.dd GGG hh:mm aaa | 2009.六月.30 公元 08:29 AM |

## 模式和区域设置

`SimpleDateFormat`类是区域敏感的。如果实例化`SimpleDateFormat`而不带`Locale`参数，它将根据默认`Locale`格式化日期和时间。模式和`Locale`都决定了格式。对于相同的模式，如果`Locale`不同，`SimpleDateFormat`可能会以不同方式格式化日期和时间。

在接下来的示例代码中，模式是硬编码在创建`SimpleDateFormat`对象的语句中的：

```java
Date today;
String result;
SimpleDateFormat formatter;

formatter = new SimpleDateFormat("EEE d MMM yy", currentLocale);
today = new Date();
result = formatter.format(today);
System.out.println("Locale: " + currentLocale.toString());
System.out.println("Result: " + result);

```

当`currentLocale`设置为不同值时，前面的代码示例生成以下输出：

```java
Locale: fr_FR
Result: mar. 30 juin 09
Locale: de_DE
Result: Di 30 Jun 09
Locale: en_US
Result: Tue 30 Jun 09

```

## 日期格式模式语法

你可以根据以下表格中的符号列表设计自己的日期和时间格式模式：

| 符号 | 含义 | 显示 | 示例 |
| --- | --- | --- | --- |
| G | 时代指示符 | 文本 | 公元 |
| y | 年份 | 数字 | 2009 |
| M | 年份中的月份 | 文本 & 数字 | 七月 & 07 |
| d | 月中的日期 | 数字 | 10 |
| h | 上午/下午的小时数 (1-12) | 数字 | 12 |
| H | 一天中的小时数 (0-23) | 数字 | 0 |
| m | 小时中的分钟数 | 数字 | 30 |
| s | 分钟中的秒数 | 数字 | 55 |
| S | 毫秒 | 数字 | 978 |
| E | 星期几 | 文本 | 星期二 |
| D | 一年中的日期 | 数字 | 189 |
| F | 月中的星期几 | 数字 | 2 (七月第二个星期三) |
| w | 年中周数 | 数字 | 27 |
| W | 月中周数 | 数字 | 2 |
| a | 上午/下午标记 | 文本 | 下午 |
| k | 一天中的小时数 (1-24) | 数字 | 24 |
| K | 上午/下午的小时数 (0-11) | 数字 | 0 |
| z | 时区 | 文本 | 太平洋标准时间 |
| ' | 转义文本 | 分隔符 | (无) |
| ' | 单引号 | 字面值 | ' |

非字母字符被视为引用文本。也就是说，即使它们没有被单引号括起来，它们也会出现在格式化文本中。

您指定的符号字母数量还决定了格式。例如，如果“zz”模式结果为“PDT”，那么“zzzz”模式会生成“太平洋夏令时间”。以下表总结了这些规则：

| 展示 | 符号数量 | 结果 |
| --- | --- | --- |
| 文本 | 1 - 3 | 缩写形式，如果存在的话 |
| 文本 | >= 4 | 完整形式 |
| 数字 | 需要的最小数字位数 | 较短的数字用零填充（例如，如果'y'的计数为 2，则年份将被截断为 2 位数） |
| 文本 & 数字 | 1 - 2 | 数字形式 |
| 文本 & 数字 | 3 | 文本形式 |
