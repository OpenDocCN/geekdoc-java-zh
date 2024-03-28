# Instant 类

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/instant.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/instant.html)

日期时间 API 的核心类之一是[`Instant`](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html)类，它表示时间轴上纳秒的开始。此类对于生成时间戳以表示机器时间很有用。

```java
import java.time.Instant;

Instant timestamp = Instant.now();

```

从`Instant`类返回的值从 1970 年 1 月 1 日的第一秒开始计时（`1970-01-01T00:00:00Z`），也称为[`EPOCH`](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#EPOCH)。在纪元之前发生的瞬间具有负值，在纪元之后发生的瞬间具有正值。

`Instant`类提供的其他常量是[`MIN`](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#MIN)，表示最小可能的（遥远的过去）瞬间，以及[`MAX`](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#MAX)，表示最大（遥远的未来）瞬间。

调用`Instant`上的`toString`会产生以下输出：

```java
2013-05-30T23:38:23.085Z

```

此格式遵循[ISO-8601](http://www.iso.org/iso/home/standards/iso8601.htm)用于表示日期和时间的标准。

`Instant`类提供了各种方法来操作`Instant`。有用于添加或减去时间的`plus`和`minus`方法。以下代码将当前时间加 1 小时：

```java
Instant oneHourLater = Instant.now().plus(1, ChronoUnit.HOURS);

```

有用于比较瞬间的方法，例如[`isAfter`](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#isAfter-java.time.Instant-)和[`isBefore`](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#isBefore-java.time.Instant-)。[`until`](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#until-java.time.temporal.Temporal-java.time.temporal.TemporalUnit-)方法返回两个`Instant`对象之间存在多少时间。以下代码报告自 Java 纪元开始以来经过了多少秒。

```java
long secondsFromEpoch = Instant.ofEpochSecond(0L).until(Instant.now(),
                        ChronoUnit.SECONDS);

```

`Instant`类不适用于年、月或日等人类时间单位。如果要在这些单位中执行计算，可以通过将`Instant`与时区绑定将`Instant`转换为另一个类，例如`LocalDateTime`或`ZonedDateTime`。然后可以访问所需单位的值。以下代码使用[`ofInstant`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html#ofInstant-java.time.Instant-java.time.ZoneId-)方法和默认时区将`Instant`转换为`LocalDateTime`对象，然后以更可读的形式打印出日期和时间：

```java
Instant timestamp;
...
LocalDateTime ldt = LocalDateTime.ofInstant(timestamp, ZoneId.systemDefault());
System.out.printf("%s %d %d at %d:%d%n", ldt.getMonth(), ldt.getDayOfMonth(),
                  ldt.getYear(), ldt.getHour(), ldt.getMinute());

```

输出将类似于以下内容：

```java
MAY 30 2013 at 18:21

```

`ZonedDateTime` 或 `OffsetTimeZone` 对象都可以转换为 `Instant` 对象，因为每个对象都映射到时间线上的确切时刻。然而，反过来却不成立。要将 `Instant` 对象转换为 `ZonedDateTime` 或 `OffsetDateTime` 对象，需要提供时区或时区偏移信息。
