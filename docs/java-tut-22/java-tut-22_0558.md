# 概述

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/overview.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/overview.html)

有两种基本表示时间的方式。一种方式以人类术语表示时间，称为*人类时间*，例如年、月、日、小时、分钟和秒。另一种方式，*机器时间*，连续测量时间沿着时间线从一个起点，称为*纪元*，以纳秒分辨率。日期时间包提供了丰富的类来表示日期和时间。日期时间 API 中的一些类旨在表示机器时间，而另一些更适合表示人类时间。

首先确定您需要日期和时间的哪些方面，然后选择满足这些需求的类或类。在选择基于时间的类时，首先确定您需要表示人类时间还是机器时间。然后确定您需要表示时间的哪些方面。您需要时区吗？日期*和*时间？仅日期？如果您需要日期，您需要月、日、*和*年，还是一个子集？

* * *

**术语：** 在日期时间 API 中捕获和处理日期或时间值的类，如`Instant`、`LocalDateTime`和`ZonedDateTime`，在本教程中统称为*基于时间的*类（或类型）。不包括支持类型，如`TemporalAdjuster`接口或`DayOfWeek`枚举。

* * *

例如，您可能会使用`LocalDate`对象来表示出生日期，因为大多数人都在同一天庆祝生日，无论他们是在出生城市还是在国际日期变更线的另一边的地球上。如果您正在跟踪占星时间，那么您可能希望使用`LocalDateTime`对象来表示出生日期和时间，或者`ZonedDateTime`，它还包括时区。如果您正在创建时间戳，那么您很可能会使用`Instant`，它允许您比较时间线上的一个瞬时点和另一个。

以下表格总结了`java.time`包中存储日期和/或时间信息的基于时间的类，或者可用于测量时间量的类。列中的勾号表示该类使用该特定类型的数据，**toString 输出**列显示使用`toString`方法打印的实例。**讨论位置**列链接到教程中相关页面。

| 类或枚举 | 年 | 月 | 日 | 小时 | 分钟 | 秒* | 时区偏移 | 时区 ID | toString 输出 | 讨论位置 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `Instant` |   |   |   |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   | `2013-08-20T15:16:26.355Z` | Instant 类 |
| `LocalDate` | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   | `2013-08-20` | 日期类 |
| `LocalDateTime` | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) |   |   | `2013-08-20T08:16:26.937` | 日期和时间类 |
| `ZonedDateTime` | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | `2013-08-21T00:16:26.941+09:00[亚洲/东京]` | 时区和偏移类 |
| `LocalTime` |   |   |   | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) |   |   | `08:16:26.943` | 日期和时间类 |
| `MonthDay` |   | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   | `--08-20` | 日期类 |
| `Year` | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |   |   | `2013` | 日期类 |
| `YearMonth` | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |   | `2013-08` | 日期类 |
| `Month` |   | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |   | `八月` | 星期几和月份枚举 |
| `OffsetDateTime` | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) | ![已选中](img/12bd475494acc0d71eb308e2275a6457.png) |   | `2013-08-20T08:16:26.954-07:00` | 时区和偏移类 |
| `OffsetTime` |   |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | `08:16:26.957-07:00` | 时区和偏移类 |
| `Duration` |   |   | ** | ** | ** | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   | `PT20H` (20 小时) | 周期和持续时间 |
| `Period` | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   | *** | *** | `P10D` (10 天) | 周期和持续时间 |
| * | 秒被捕捉到纳秒精度。 |
| ** | 这个类不存储这些信息，但有方法提供这些单位的时间。 |
| *** | 当`Period`添加到`ZonedDateTime`时，会考虑夏令时或其他本地时间差异。 |
