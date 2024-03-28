# 日期和时间类

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/datetime.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/datetime.html)

## LocalTime

[`LocalTime`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalTime.html)类类似于其他以`Local`为前缀的类，但仅处理时间。这个类非常适用于表示人类时间，比如电影时间，或者当地图书馆的开放和关闭时间。它也可以用来创建数字时钟，就像下面的例子所示：

```java
LocalTime thisSec;

for (;;) {
    thisSec = LocalTime.now();

    // implementation of display code is left to the reader
    display(thisSec.getHour(), thisSec.getMinute(), thisSec.getSecond());
}

```

`LocalTime`类不存储时区或夏令时信息。

## LocalDateTime

处理日期和时间，不带时区的类是[`LocalDateTime`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)，是日期时间 API 的核心类之一。这个类用于表示日期（月-日-年）和时间（时-分-秒-纳秒），实际上是`LocalDate`和`LocalTime`的组合。这个类可以用来表示特定事件，比如 2013 年 8 月 17 日下午 1:10 开始的美洲杯挑战者系列赛首场路易威登杯决赛。请注意，这意味着当地时间下午 1:10。要包含时区，必须使用`ZonedDateTime`或`OffsetDateTime`，如时区和偏移类中所讨论的。

除了每个基于时间的类都提供的`now`方法外，`LocalDateTime`类还有各种`of`方法（或以`of`为前缀的方法）来创建`LocalDateTime`的实例。还有一个`from`方法，用于将另一种时间格式的实例转换为`LocalDateTime`实例。还有用于添加或减去小时、分钟、天、周和月份的方法。下面的例子展示了其中一些方法。日期时间表达式用粗体表示：

```java
System.out.printf("now: %s%n", LocalDateTime.now());

System.out.printf("Apr 15, 1994 @ 11:30am: %s%n",
                  LocalDateTime.of(1994, Month.APRIL, 15, 11, 30));

System.out.printf("now (from Instant): %s%n",
                  LocalDateTime.ofInstant(Instant.now(), ZoneId.systemDefault()));

System.out.printf("6 months from now: %s%n",
                  LocalDateTime.now().plusMonths(6));

System.out.printf("6 months ago: %s%n",
                  LocalDateTime.now().minusMonths(6));

```

这段代码产生的输出看起来类似于以下内容：

```java
now: 2013-07-24T17:13:59.985
Apr 15, 1994 @ 11:30am: 1994-04-15T11:30
now (from Instant): 2013-07-24T17:14:00.479
6 months from now: 2014-01-24T17:14:00.480
6 months ago: 2013-01-24T17:14:00.481

```
