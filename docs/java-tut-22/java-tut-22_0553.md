# 教训：日期时间概述

> 原文：[`docs.oracle.com/javase/tutorial/datetime/overview/index.html`](https://docs.oracle.com/javase/tutorial/datetime/overview/index.html)

时间似乎是一个简单的主题；即使是一只廉价的手表也可以提供相当准确的日期和时间。然而，仔细观察后，你会意识到时间的微妙复杂性和许多影响你对时间理解的因素。例如，将一个月加到 1 月 31 日的结果对于闰年和其他年份是不同的。时区也增加了复杂性。例如，一个国家可能会在短时间内进入和退出夏令时，或者一年内多次进入和退出夏令时，或者在某一年完全跳过夏令时。

日期时间 API 使用[ISO-8601](http://www.iso.org/iso/home/standards/iso8601.htm)中定义的日历系统作为默认日历。这个日历基于格里高利历系统，在全球范围内被用作代表日期和时间的事实标准。日期时间 API 中的核心类的名称如`LocalDateTime`、`ZonedDateTime`和`OffsetDateTime`。所有这些类都使用 ISO 日历系统。如果你想使用另一个日历系统，比如伊斯兰历或泰国佛历，`java.time.chrono`包允许你使用其中一个预定义的日历系统。或者你也可以创建自己的日历系统。

日期时间 API 使用[Unicode 通用区域数据存储库 (CLDR)](http://cldr.unicode.org)。这个存储库支持世界上的语言，并包含可用的最大的区域数据集合。这个存储库中的信息已被本地化到数百种语言。日期时间 API 还使用[时区数据库 (TZDB)](http://www.iana.org/time-zones)。这个数据库提供自 1970 年以来全球每个时区变更的信息，以及自该概念引入以来主要时区的历史。
