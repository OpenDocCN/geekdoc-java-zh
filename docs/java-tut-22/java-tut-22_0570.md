# 非 ISO 日期转换

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/nonIso.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/nonIso.html)

本教程不会详细讨论[`java.time.chrono`](https://docs.oracle.com/javase/8/docs/api/java/time/chrono/package-summary.html)包。但是，了解到该包提供了几个不基于 ISO 的预定义年表，如日本、伊斯兰、民国和泰国佛教。您还可以使用此包创建自己的年表。

本节将向您展示如何在 ISO 日期和其他预定义年表日期之间进行转换。

## 转换为非 ISO 日期

您可以使用`from(TemporalAccessor)`方法将 ISO 日期转换为其他年表的日期，例如[`JapaneseDate.from(TemporalAccessor)`](https://docs.oracle.com/javase/8/docs/api/java/time/chrono/JapaneseDate.html#from-java.time.temporal.TemporalAccessor-)。如果无法将日期转换为有效实例，则此方法会抛出`DateTimeException`。以下代码将`LocalDateTime`实例转换为几个预定义的非 ISO 日历日期：

```java
LocalDateTime date = LocalDateTime.of(2013, Month.JULY, 20, 19, 30);
JapaneseDate jdate     = JapaneseDate.from(date);
HijrahDate hdate       = HijrahDate.from(date);
MinguoDate mdate       = MinguoDate.from(date);
ThaiBuddhistDate tdate = ThaiBuddhistDate.from(date);

```

``StringConverter``示例将从`LocalDate`转换为`ChronoLocalDate`再转换为`String`，然后再转回去。`toString`方法接受`LocalDate`实例和`Chronology`，并通过提供的`Chronology`返回转换后的字符串。`DateTimeFormatterBuilder`用于构建可用于打印日期的字符串：

```java
/**
 * Converts a LocalDate (ISO) value to a ChronoLocalDate date
 * using the provided Chronology, and then formats the
 * ChronoLocalDate to a String using a DateTimeFormatter with a
 * SHORT pattern based on the Chronology and the current Locale.
 *
 * @param localDate - the ISO date to convert and format.
 * @param chrono - an optional Chronology. If null, then IsoChronology is used.
 */
public static String toString(LocalDate localDate, Chronology chrono) {
    if (localDate != null) {
        Locale locale = Locale.getDefault(Locale.Category.FORMAT);
        ChronoLocalDate cDate;
        if (chrono == null) {
            chrono = IsoChronology.INSTANCE;
        }
        try {
            cDate = chrono.date(localDate);
        } catch (DateTimeException ex) {
            System.err.println(ex);
            chrono = IsoChronology.INSTANCE;
            cDate = localDate;
        }
        DateTimeFormatter dateFormatter =
            DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT)
                             .withLocale(locale)
                             .withChronology(chrono)
                             .withDecimalStyle(DecimalStyle.of(locale));
        String pattern = "M/d/yyyy GGGGG";
        return dateFormatter.format(cDate);
    } else {
        return "";
    }
}

```

当使用以下日期调用预定义年表的方法时：

```java
LocalDate date = LocalDate.of(1996, Month.OCTOBER, 29);
System.out.printf("%s%n",
     StringConverter.toString(date, JapaneseChronology.INSTANCE));
System.out.printf("%s%n",
     StringConverter.toString(date, MinguoChronology.INSTANCE));
System.out.printf("%s%n",
     StringConverter.toString(date, ThaiBuddhistChronology.INSTANCE));
System.out.printf("%s%n",
     StringConverter.toString(date, HijrahChronology.INSTANCE));

```

输出如下：

```java
10/29/0008 H
10/29/0085 1
10/29/2539 B.E.
6/16/1417 1

```

## 转换为基于 ISO 的日期

您可以使用静态[`LocalDate.from`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#from-java.time.temporal.TemporalAccessor-)方法将非 ISO 日期转换为`LocalDate`实例，如下例所示：

```java
LocalDate date = LocalDate.from(JapaneseDate.now());

```

其他基于时间的类也提供此方法，如果日期无法转换，则会抛出`DateTimeException`。

来自``StringConverter``示例的`fromString`方法解析包含非 ISO 日期的`String`并返回`LocalDate`实例。

```java
/**
 * Parses a String to a ChronoLocalDate using a DateTimeFormatter
 * with a short pattern based on the current Locale and the
 * provided Chronology, then converts this to a LocalDate (ISO)
 * value.
 *
 * @param text   - the input date text in the SHORT format expected
 *                 for the Chronology and the current Locale.
 *
 * @param chrono - an optional Chronology. If null, then IsoChronology
 *                 is used.
 */
public static LocalDate fromString(String text, Chronology chrono) {
    if (text != null && !text.isEmpty()) {
        Locale locale = Locale.getDefault(Locale.Category.FORMAT);
        if (chrono == null) {
           chrono = IsoChronology.INSTANCE;
        }
        String pattern = "M/d/yyyy GGGGG";
        DateTimeFormatter df = new DateTimeFormatterBuilder().parseLenient()
                              .appendPattern(pattern)
                              .toFormatter()
                              .withChronology(chrono)
                              .withDecimalStyle(DecimalStyle.of(locale));
        TemporalAccessor temporal = df.parse(text);
        ChronoLocalDate cDate = chrono.date(temporal);
        return LocalDate.from(cDate);
    }
return null;
}

```

当使用以下字符串调用方法时：

```java
System.out.printf("%s%n", StringConverter.fromString("10/29/0008 H",
    JapaneseChronology.INSTANCE));
System.out.printf("%s%n", StringConverter.fromString("10/29/0085 1",
    MinguoChronology.INSTANCE));
System.out.printf("%s%n", StringConverter.fromString("10/29/2539 B.E.",
    ThaiBuddhistChronology.INSTANCE));
System.out.printf("%s%n", StringConverter.fromString("6/16/1417 1",
    HijrahChronology.INSTANCE));

```

打印的字符串应该都能转换回 1996 年 10 月 29 日：

```java
1996-10-29
1996-10-29
1996-10-29
1996-10-29

```
