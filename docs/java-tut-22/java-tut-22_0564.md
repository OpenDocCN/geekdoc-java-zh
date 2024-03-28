# 解析和格式化

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/format.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/format.html)

日期时间 API 中基于时间的类提供了用于解析包含日期和时间信息的字符串的`parse`方法。这些类还提供了用于为显示格式化基于时间的对象的`format`方法。在这两种情况下，过程是相似的：您提供一个模式给`DateTimeFormatter`来创建一个格式化程序对象。然后将此格式化程序传递给`parse`或`format`方法。

`DateTimeFormatter` 类提供了许多[预定义的格式化程序](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#predefined)，或者您可以定义自己的格式化程序。

如果在转换过程中出现问题，`parse` 和 `format` 方法会抛出异常。因此，您的解析代码应捕获`DateTimeParseException`错误，您的格式化代码应捕获`DateTimeException`错误。有关异常处理的更多信息，请参阅捕获和处理异常。

`DateTimeFormatter` 类既是不可变的又是线程安全的；在适当的情况下，应该将其分配给静态常量。

* * *

**版本说明：** `java.time` 日期时间对象可以直接与`java.util.Formatter`和`String.format`一起使用，方法是使用与旧的`java.util.Date`和`java.util.Calendar`类一起使用的熟悉基于模式的格式化。

* * *

## 解析

`LocalDate` 类中的单参数[`parse(CharSequence)`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#parse-java.lang.CharSequence-)方法使用`ISO_LOCAL_DATE`格式化程序。要指定不同的格式化程序，可以使用两个参数的[`parse(CharSequence, DateTimeFormatter)`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#parse-java.lang.CharSequence-java.time.format.DateTimeFormatter-)方法。以下示例使用预定义的`BASIC_ISO_DATE`格式化程序，该格式使用`19590709`表示 1959 年 7 月 9 日。

```java
String in = ...;
LocalDate date = LocalDate.parse(in, DateTimeFormatter.BASIC_ISO_DATE);

```

您还可以使用自己的模式定义格式化程序。以下代码来自``Parse``示例，创建了一个应用格式为"MMM d yyyy"的格式化程序。该格式指定三个字符表示月份，一个数字表示日期，四个数字表示年份。使用此模式创建的格式化程序将识别诸如"Jan 3 2003"或"Mar 23 1994"之类的字符串。但是，要将格式指定为"MMM dd yyyy"，日期的两个字符，则您必须始终使用两个字符，对于一位数日期，用零填充："Jun 03 2003"。

```java
String input = ...;
try {
    DateTimeFormatter formatter =
                      DateTimeFormatter.ofPattern("MMM d yyyy");
    LocalDate date = LocalDate.parse(input, formatter);
    System.out.printf("%s%n", date);
}
catch (DateTimeParseException exc) {
    System.out.printf("%s is not parsable!%n", input);
    throw exc;      // Rethrow the exception.
}
// 'date' has been successfully parsed

```

`DateTimeFormatter` 类的文档指定了您可以使用的[符号的完整列表](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#patterns)来指定格式化或解析的模式。

非 ISO 日期转换页面上的`StringConverter`示例提供了另一个日期格式化的示例。

## 格式化

[`format(DateTimeFormatter)`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#format-java.time.format.DateTimeFormatter-) 方法使用指定的格式将基于时间的对象转换为字符串表示。以下代码来自 ``Flight`` 示例，使用格式"MMM d yyy hh:mm a"转换了一个`ZonedDateTime`实例。日期的定义方式与之前的解析示例相同，但此模式还包括小时、分钟以及上午和下午组件。

```java
ZoneId leavingZone = ...;
ZonedDateTime departure = ...;

try {
    DateTimeFormatter format = DateTimeFormatter.ofPattern("MMM d yyyy  hh:mm a");
    String out = departure.format(format);
    System.out.printf("LEAVING:  %s (%s)%n", out, leavingZone);
}
catch (DateTimeException exc) {
    System.out.printf("%s can't be formatted!%n", departure);
    throw exc;
}

```

这个示例的输出，打印了到达和离开时间，如下所示：

```java
LEAVING:  Jul 20 2013  07:30 PM (America/Los_Angeles)
ARRIVING: Jul 21 2013  10:20 PM (Asia/Tokyo)

```
