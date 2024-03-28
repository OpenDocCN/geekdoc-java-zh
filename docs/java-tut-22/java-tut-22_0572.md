# 摘要

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/summary.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/summary.html)

`java.time` 包含许多类，您的程序可以使用这些类来表示时间和日期。这是一个非常丰富的 API。基于 ISO 的日期的关键入口点如下：

+   `Instant` 类提供了时间线的机器视图。

+   `LocalDate`、`LocalTime` 和 `LocalDateTime` 类提供了日期和时间的人类视图，没有任何关于时区的参考。

+   `ZoneId`、`ZoneRules` 和 `ZoneOffset` 类描述时区、时区偏移和时区规则。

+   `ZonedDateTime` 类表示带有时区的日期和时间。`OffsetDateTime` 和 `OffsetTime` 类分别表示日期和时间，或仅时间。这些类考虑了时区偏移。

+   `Duration` 类以秒和纳秒来衡量时间量。

+   `Period` 类使用年、月和日来衡量时间量。

其他非 ISO 日历系统可以使用 `java.time.chrono` 包来表示。尽管本教程不涵盖此包，但 非 ISO 日期转换 页面提供了关于将基于 ISO 的日期转换为其他日历系统的信息。

日期时间 API 是作为 Java 社区流程的一部分开发的，其编号为 JSR 310。更多信息，请参阅 [JSR 310: 日期和时间 API](http://jcp.org/en/jsr/detail?id=310)。
