# 时间包

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/temporal.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/temporal.html)

[`java.time.temporal`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/package-summary.html)包提供了支持日期和时间代码的一组接口、类和枚举，特别是日期和时间计算。

这些接口旨在在最低级别使用。典型的应用代码应该根据具体类型（如`LocalDate`或`ZonedDateTime`）声明变量和参数，而不是根据`Temporal`接口。这与声明类型为`String`而不是`CharSequence`的变量完全相同。

## 时间和 TemporalAccessor

[`Temporal`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/Temporal.html)接口提供了访问基于时间的对象的框架，并由基于时间的类（如`Instant`、`LocalDateTime`和`ZonedDateTime`）实现。该接口提供了添加或减去时间单位的方法，使得基于时间的算术在各种日期和时间类之间变得简单和一致。[`TemporalAccessor`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAccessor.html)接口提供了`Temporal`接口的只读版本。

`Temporal`和`TemporalAccessor`对象都是根据字段定义的，如[`TemporalField`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalField.html)接口中所指定的。[`ChronoField`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoField.html)枚举是`TemporalField`接口的具体实现，并提供了一组定义的常量，如`DAY_OF_WEEK`、`MINUTE_OF_HOUR`和`MONTH_OF_YEAR`。

这些字段的单位由[`TemporalUnit`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalUnit.html)接口指定。`ChronoUnit`枚举实现了`TemporalUnit`接口。字段`ChronoField.DAY_OF_WEEK`是`ChronoUnit.DAYS`和`ChronoUnit.WEEKS`的组合。`ChronoField`和`ChronoUnit`枚举在以下部分讨论。

`Temporal`接口中基于算术的方法需要根据[`TemporalAmount`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAmount.html)值定义的参数。`Period`和`Duration`类（在 Period and Duration 中讨论）实现了`TemporalAmount`接口。

## ChronoField 和 IsoFields

实现了`TemporalField`接口的[`ChronoField`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoField.html)枚举提供了一组丰富的常量，用于访问日期和时间值。一些示例包括`CLOCK_HOUR_OF_DAY`、`NANO_OF_DAY`和`DAY_OF_YEAR`。这个枚举可以用来表达时间的概念方面，比如一年中的第三周、一天中的第 11 小时或一个月中的第一个星期一。当你遇到一个未知类型的`Temporal`时，可以使用[`TemporalAccessor.isSupported(TemporalField)`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAccessor.html#isSupported-java.time.temporal.TemporalField-)方法来确定`Temporal`是否支持特定字段。下面这行代码返回`false`，表示`LocalDate`不支持`ChronoField.CLOCK_HOUR_OF_DAY`：

```java
boolean isSupported = LocalDate.now().isSupported(ChronoField.CLOCK_HOUR_OF_DAY);

```

针对 ISO-8601 日历系统的特定字段在[`IsoFields`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/IsoFields.html)类中定义。以下示例展示了如何使用`ChronoField`和`IsoFields`获取字段的值：

```java
time.get(ChronoField.MILLI_OF_SECOND)
int qoy = date.get(IsoFields.QUARTER_OF_YEAR);

```

另外两个类定义了可能有用的额外字段，[`WeekFields`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/WeekFields.html) 和 [`JulianFields`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/JulianFields.html)。

## ChronoUnit

实现了`TemporalUnit`接口的[`ChronoUnit`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoUnit.html)枚举提供了一组基于日期和时间的标准单位，从毫秒到千年。请注意，并非所有的`ChronoUnit`对象都被所有类支持。例如，`Instant`类不支持`ChronoUnit.MONTHS`或`ChronoUnit.YEARS`。日期时间 API 中的类包含`isSupported(TemporalUnit)`方法，可用于验证一个类是否支持特定的时间单位。下面对`isSupported`的调用返回`false`，确认`Instant`类不支持`ChronoUnit.DAYS`。

```java
Instant instant = Instant.now();
boolean isSupported = instant.isSupported(ChronoUnit.DAYS);

```
