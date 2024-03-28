# DayOfWeek 和 Month 枚举

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/enum.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/enum.html)

日期时间 API 提供了用于指定星期几和每年月份的枚举。

## DayOfWeek

[`DayOfWeek`](https://docs.oracle.com/javase/8/docs/api/java/time/DayOfWeek.html)枚举由描述一周中每天的七个常量组成：`MONDAY`到`SUNDAY`。`DayOfWeek`常量的整数值范围从 1（星期一）到 7（星期日）。使用定义的常量（`DayOfWeek.FRIDAY`）可以使您的代码更易读。

这个枚举还提供了许多方法，类似于时间为基础的类提供的方法。例如，以下代码将“Monday”加 3 天并打印结果。输出为“THURSDAY”：

```java
System.out.printf("%s%n", DayOfWeek.MONDAY.plus(3));

```

使用[`getDisplayName(TextStyle, Locale)`](https://docs.oracle.com/javase/8/docs/api/java/time/DayOfWeek.html#getDisplayName-java.time.format.TextStyle-java.util.Locale-)方法，您可以检索一个字符串来标识用户所在地区的星期几。[`TextStyle`](https://docs.oracle.com/javase/8/docs/api/java/time/format/TextStyle.html)枚举允许您指定要显示的字符串类型：`FULL`、`NARROW`（通常是一个字母）或`SHORT`（缩写）。`STANDALONE`的`TextStyle`常量在某些语言中使用，当作为日期的一部分时输出不同于单独使用时的输出。以下示例打印了“Monday”三种主要形式的`TextStyle`：

```java
DayOfWeek dow = DayOfWeek.MONDAY;
Locale locale = Locale.getDefault();
System.out.println(dow.getDisplayName(TextStyle.FULL, locale));
System.out.println(dow.getDisplayName(TextStyle.NARROW, locale));
System.out.println(dow.getDisplayName(TextStyle.SHORT, locale));

```

此代码在`en`区域设置下的输出如下：

```java
Monday
M
Mon

```

## 月

[`Month`](https://docs.oracle.com/javase/8/docs/api/java/time/Month.html)枚举包括了十二个月的常量，从`JANUARY`到`DECEMBER`。与`DayOfWeek`枚举一样，`Month`枚举是强类型的，每个常量的整数值对应 ISO 范围从 1（一月）到 12（十二月）。使用定义的常量（`Month.SEPTEMBER`）可以使您的代码更易读。

`Month`枚举还包括许多方法。以下代码使用`maxLength`方法打印二月份最大可能的天数。输出为“29”：

```java
System.out.printf("%d%n", Month.FEBRUARY.maxLength());

```

`Month`枚举还实现了[`getDisplayName(TextStyle, Locale)`](https://docs.oracle.com/javase/8/docs/api/java/time/Month.html#getDisplayName-java.time.format.TextStyle-java.util.Locale-)方法，使用指定的`TextStyle`检索一个字符串来标识用户所在地区的月份。如果特定的`TextStyle`未定义，则返回表示常量数值的字符串。以下代码使用三种主要文本样式打印八月份的月份：

```java
Month month = Month.AUGUST;
Locale locale = Locale.getDefault();
System.out.println(month.getDisplayName(TextStyle.FULL, locale));
System.out.println(month.getDisplayName(TextStyle.NARROW, locale));
System.out.println(month.getDisplayName(TextStyle.SHORT, locale));

```

此代码在`en`区域设置下的输出如下：

```java
August
A
Aug

```
