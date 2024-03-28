# 传统日期时间代码

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/legacy.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/legacy.html)

在 Java SE 8 发布之前，Java 日期和时间机制由 [`java.util.Date`](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html)、[`java.util.Calendar`](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html) 和 [`java.util.TimeZone`](https://docs.oracle.com/javase/8/docs/api/java/util/TimeZone.html) 类以及它们的子类，如 [`java.util.GregorianCalendar`](https://docs.oracle.com/javase/8/docs/api/java/util/GregorianCalendar.html) 提供。这些类有几个缺点，包括：

+   `Calendar` 类不是类型安全的。

+   由于这些类是可变的，它们不能在多线程应用程序中使用。

+   应用程序代码中常见的错误是由于月份编号不寻常和缺乏类型安全性。

## 与传统代码的互操作性

也许您有使用 `java.util` 日期和时间类的传统代码，并且希望在最小更改代码的情况下利用 `java.time` 功能。

JDK 8 发布中添加了几种允许在 `java.util` 和 `java.time` 对象之间进行转换的方法：

+   [`Calendar.toInstant()`](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html#toInstant--) 将 `Calendar` 对象转换为 `Instant`。

+   [`GregorianCalendar.toZonedDateTime()`](https://docs.oracle.com/javase/8/docs/api/java/util/GregorianCalendar.html#toZonedDateTime--) 将 `GregorianCalendar` 实例转换为 `ZonedDateTime`。

+   [`GregorianCalendar.from(ZonedDateTime)`](https://docs.oracle.com/javase/8/docs/api/java/util/GregorianCalendar.html#from-java.time.ZonedDateTime-) 使用默认区域设置从 `ZonedDateTime` 实例创建一个 `GregorianCalendar` 对象。

+   [`Date.from(Instant)`](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html#from-java.time.Instant-) 从 `Instant` 创建一个 `Date` 对象。

+   [`Date.toInstant()`](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html#toInstant--) 将 `Date` 对象转换为 `Instant`。

+   [`TimeZone.toZoneId()`](https://docs.oracle.com/javase/8/docs/api/java/util/TimeZone.html#toZoneId--) 将一个 `TimeZone` 对象转换为 `ZoneId`。

以下示例将 `Calendar` 实例转换为 `ZonedDateTime` 实例。请注意，必须提供一个时区来从 `Instant` 转换为 `ZonedDateTime`：

```java
Calendar now = Calendar.getInstance();
ZonedDateTime zdt = ZonedDateTime.ofInstant(now.toInstant(), ZoneId.systemDefault()));

```

以下示例展示了 `Date` 和 `Instant` 之间的转换：

```java
Instant inst = date.toInstant();

Date newDate = Date.from(inst);

```

以下示例将从 `GregorianCalendar` 转换为 `ZonedDateTime`，然后从 `ZonedDateTime` 转换为 `GregorianCalendar`。其他基于时间的类是使用 `ZonedDateTime` 实例创建的：

```java
GregorianCalendar cal = ...;

TimeZone tz = cal.getTimeZone();
int tzoffset = cal.get(Calendar.ZONE_OFFSET);

ZonedDateTime zdt = cal.toZonedDateTime();

GregorianCalendar newCal = GregorianCalendar.from(zdt);

LocalDateTime ldt = zdt.toLocalDateTime();
LocalDate date = zdt.toLocalDate();
LocalTime time = zdt.toLocalTime();

```

## 将 java.util 日期和时间功能映射到 java.time

由于在 Java SE 8 发布中完全重新设计了日期和时间的 Java 实现，您不能将一个方法替换为另一个方法。如果您想使用 `java.time` 包提供的丰富功能，最简单的解决方案是使用前一节中列出的 `toInstant` 或 `toZonedDateTime` 方法。但是，如果您不想使用该方法或该方法不满足您的需求，则必须重写您的日期时间代码。

在 概述 页面介绍的表格是评估哪些 `java.time` 类满足您需求的好地方。

两个 API 之间没有一对一的映射对应关系，但以下表格给出了 `java.util` 日期和时间类中的哪些功能映射到 `java.time` API 的一般想法。

| java.util 功能 | java.time 功能 | 注释 |
| --- | --- | --- |

| `java.util.Date` | `java.time.Instant` | `Instant` 和 `Date` 类相似。每个类：- 表示时间线上的瞬时时间点（UTC）

- 持有独立于时区的时间

- 表示为自 1970-01-01T00:00:00Z 以来的纪元秒数加上纳秒

`Date.from(Instant)` 和 `Date.toInstant()` 方法允许在这些类之间进行转换。 |

| `java.util.GregorianCalendar` | `java.time.ZonedDateTime` | `ZonedDateTime` 类是 `GregorianCalendar` 的替代品。它提供以下类似功能。人类时间表示如下：

`LocalDate`：年，月，日

`LocalTime`：小时，分钟，秒，纳秒

`ZoneId`：时区

`ZoneOffset`：与 GMT 的当前偏移量

`GregorianCalendar.from(ZonedDateTime)` 和 `GregorianCalendar.to(ZonedDateTime)` 方法促进这些类之间的转换。

| `java.util.TimeZone` | `java.time.ZoneId` 或 `java.time.ZoneOffset` | `ZoneId` 类指定时区标识符，并访问每个时区使用的规则。`ZoneOffset` 类仅指定与格林威治/UTC 的偏移量。更多信息，请参阅时区和偏移类。 |
| --- | --- | --- |
| 将日期设置为 `1970-01-01` 的 `GregorianCalendar` | `java.time.LocalTime` | 将日期设置为 `1970-01-01` 的 `GregorianCalendar` 实例中的代码以使用时间组件可以替换为 `LocalTime` 实例。 |
| `GregorianCalendar` 时间设置为 `00:00.` | `java.time.LocalDate` | 将时间设置为 `GregorianCalendar` 实例中的 `00:00` 以使用日期组件的代码可以替换为 `LocalDate` 实例。（这种 `GregorianCalendar` 方法存在缺陷，因为由于转换到夏令时，有些国家一年中不会发生午夜。） |

## 日期和时间格式化

尽管`java.time.format.DateTimeFormatter`提供了一个强大的机制来格式化日期和时间值，但你也可以直接使用`java.time`基于时间的类与`java.util.Formatter`和`String.format`一起使用，使用与`java.util`日期和时间类相同的基于模式的格式化。
