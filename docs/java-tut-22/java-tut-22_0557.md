# 课程：标准日历

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/index.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/index.html)

日期时间 API 的核心是[java.time](https://docs.oracle.com/javase/8/docs/api/java/time/package-summary.html)包。`java.time`中定义的类基于 ISO 日历系统，这是代表日期和时间的世界标准。ISO 日历遵循普罗列普提克格里高利规则。格里高利日历于 1582 年引入；在*普罗列普提克*格里高利日历中，日期从那时开始向前延伸，以创建一致的、统一的时间线，并简化日期计算。

本课程涵盖以下主题：

## 概述

这一部分比较了人类时间和机器时间的概念，并提供了`java.time`包中主要基于时间的类的表格。

## 星期几和月份枚举

这一部分讨论了定义星期几（`DayOfWeek`）和定义月份（`Month`）的枚举。

## 日期类

这一部分展示了仅处理日期而不涉及时间或时区的基于时间的类。这四个类分别是`LocalDate`、`YearMonth`、`MonthDay`和`Year`。

## 日期和时间类

这一部分介绍了处理时间和日期时间的`LocalTime`和`LocalDateTime`类，但不涉及时区。

## 时区和偏移类

这一部分讨论了存储时区（或时区偏移）信息的基于时间的类，`ZonedDateTime`、`OffsetDateTime`和`OffsetTime`。还讨论了支持类`ZoneId`、`ZoneRules`和`ZoneOffset`。

## Instant 类

这一部分讨论了`Instant`类，它表示时间轴上的瞬时时刻。

## 解析和格式化

这一部分概述了如何使用预定义的格式化程序来格式化和解析日期和时间值。

## 时间包

这一部分概述了`java.time.temporal`包，支持时间类、字段（`TemporalField`和`ChronoField`）和单位（`TemporalUnit`和`ChronoUnit`）。这一部分还解释了如何使用时间调整器来检索调整后的时间值，比如“4 月 11 日后的第一个星期二”，以及如何执行时间查询。

## 周期和持续时间

这一部分解释了如何计算一段时间，使用了`Period`和`Duration`类，以及`ChronoUnit.between`方法。

## 时钟

这一部分简要介绍了`Clock`类。您可以使用这个类来提供一个替代时钟来替代系统时钟。

## 非 ISO 日期转换

这一部分解释了如何将 ISO 日历系统中的日期转换为非 ISO 日历系统中的日期，比如`JapaneseDate`或`ThaiBuddhistDate`。

## 遗留日期时间代码

本节提供了一些关于如何将旧的`java.util.Date`和`java.util.Calendar`代码转换为日期时间 API 的技巧。

## 摘要

本节提供了标准日历课程的摘要。
