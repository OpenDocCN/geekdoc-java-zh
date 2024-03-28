# 日期类

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/date.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/date.html)

日期时间 API 提供了四个专门处理日期信息的类，不考虑时间或时区。这些类的使用建议由类名决定：`LocalDate`、`YearMonth`、`MonthDay`和`Year`。

## LocalDate

[`LocalDate`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html)表示 ISO 日历中的年-月-日，用于表示没有时间的日期。您可以使用`LocalDate`来跟踪重要事件，如出生日期或结婚日期。以下示例使用`of`和`with`方法创建`LocalDate`实例：

```java
LocalDate date = LocalDate.of(2000, Month.NOVEMBER, 20);
LocalDate nextWed = date.with(TemporalAdjusters.next(DayOfWeek.WEDNESDAY));

```

有关`TemporalAdjuster`接口的更多信息，请参见时间调整器。

除了通常的方法外，`LocalDate`类还提供了用于获取有关给定日期信息的 getter 方法。[`getDayOfWeek`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#getDayOfWeek--)方法返回特定日期所在的星期几。例如，以下代码返回"MONDAY"：

```java
DayOfWeek dotw = LocalDate.of(2012, Month.JULY, 9).getDayOfWeek();

```

以下示例使用`TemporalAdjuster`来获取特定日期后的第一个星期三。

```java
LocalDate date = LocalDate.of(2000, Month.NOVEMBER, 20);
TemporalAdjuster adj = TemporalAdjusters.next(DayOfWeek.WEDNESDAY);
LocalDate nextWed = date.with(adj);
System.out.printf("For the date of %s, the next Wednesday is %s.%n",
                  date, nextWed);

```

运行代码会产生以下结果：

```java
For the date of 2000-11-20, the next Wednesday is 2000-11-22.

```

Period and Duration 部分还有使用`LocalDate`类的示例。

## YearMonth

[`YearMonth`](https://docs.oracle.com/javase/8/docs/api/java/time/YearMonth.html)类表示特定年份的月份。以下示例使用`YearMonth.lengthOfMonth()`方法来确定几个年份和月份组合的天数。

```java
YearMonth date = YearMonth.now();
System.out.printf("%s: %d%n", date, date.lengthOfMonth());

YearMonth date2 = YearMonth.of(2010, Month.FEBRUARY);
System.out.printf("%s: %d%n", date2, date2.lengthOfMonth());

YearMonth date3 = YearMonth.of(2012, Month.FEBRUARY);
System.out.printf("%s: %d%n", date3, date3.lengthOfMonth());

```

此代码的输出如下：

```java
2013-06: 30
2010-02: 28
2012-02: 29

```

## MonthDay

[`MonthDay`](https://docs.oracle.com/javase/8/docs/api/java/time/MonthDay.html)类表示特定月份的某一天，例如 1 月 1 日的元旦。

以下示例使用[`MonthDay.isValidYear`](https://docs.oracle.com/javase/8/docs/api/java/time/MonthDay.html#isValidYear-int-)方法来确定 2010 年 2 月 29 日是否有效。调用返回`false`，确认 2010 年不是闰年。

```java
MonthDay date = MonthDay.of(Month.FEBRUARY, 29);
boolean validLeapYear = date.isValidYear(2010);

```

## Year

[`Year`](https://docs.oracle.com/javase/8/docs/api/java/time/Year.html)类表示一年。以下示例使用[`Year.isLeap`](https://docs.oracle.com/javase/8/docs/api/java/time/Year.html#isLeap--)方法来确定给定年份是否是闰年。调用返回`true`，确认 2012 年是闰年。

```java
boolean validLeapYear = Year.of(2012).isLeap();

```
