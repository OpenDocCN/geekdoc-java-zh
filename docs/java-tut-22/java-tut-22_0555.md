# 日期时间包

> 原文：[`docs.oracle.com/javase/tutorial/datetime/overview/packages.html`](https://docs.oracle.com/javase/tutorial/datetime/overview/packages.html)

日期时间 API 由主要包`java.time`和四个子包组成：

`java.time`

该 API 的核心用于表示日期和时间。它包括了日期、时间、日期和时间的组合、时区、瞬间、持续时间和时钟的类。这些类基于 ISO-8601 中定义的日历系统，是不可变的和线程安全的。

`java.time.chrono`

用于表示除默认 ISO-8601 之外的日历系统的 API。您也可以定义自己的日历系统。本教程不会详细介绍这个包。

`java.time.format`

用于格式化和解析日期和时间的类。

`java.time.temporal`

扩展 API，主要用于框架和库的编写者，允许日期和时间类之间的互操作，查询和调整。在这个包中定义了字段（`TemporalField` 和 `ChronoField`）和单位（`TemporalUnit` 和 `ChronoUnit`）。

`java.time.zone`

支持时区、时区偏移和时区规则的类。如果涉及时区，大多数开发人员只需要使用`ZonedDateTime`、`ZoneId`或`ZoneOffset`。
