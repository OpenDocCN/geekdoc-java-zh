# 时间调整器

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/adjusters.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/adjusters.html)

[`TemporalAdjuster`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAdjuster.html)接口，位于`java.time.temporal`包中，提供了接受`Temporal`值并返回调整后值的方法。这些调整器可以与任何基于时间的类型一起使用。

如果将调整器与`ZonedDateTime`一起使用，则会计算出一个新日期，保留原始的时间和时区值。

## 预定义调整器

[`TemporalAdjusters`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAdjusters.html) 类（注意复数形式）提供了一组预定义的调整器，用于查找月份的第一天或最后一天，年份的第一天或最后一天，月份的最后一个星期三，或特定日期后的第一个星期二等等。这些预定义的调整器被定义为静态方法，并设计用于与静态导入语句一起使用。

以下示例结合了几个`TemporalAdjusters`方法，与基于时间的类中定义的`with`方法一起，根据 2000 年 10 月 15 日的原始日期计算新日期：

```java
LocalDate date = LocalDate.of(2000, Month.OCTOBER, 15);
DayOfWeek dotw = date.getDayOfWeek();
System.out.printf("%s is on a %s%n", date, dotw);

System.out.printf("first day of Month: %s%n",
                  date.with(TemporalAdjusters.firstDayOfMonth()));
System.out.printf("first Monday of Month: %s%n",
                  date.with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY)));
System.out.printf("last day of Month: %s%n",
                  date.with(TemporalAdjusters.lastDayOfMonth()));
System.out.printf("first day of next Month: %s%n",
                  date.with(TemporalAdjusters.firstDayOfNextMonth()));
System.out.printf("first day of next Year: %s%n",
                  date.with(TemporalAdjusters.firstDayOfNextYear()));
System.out.printf("first day of Year: %s%n",
                  date.with(TemporalAdjusters.firstDayOfYear()));

```

这将产生以下输出：

```java
2000-10-15 is on a SUNDAY
first day of Month: 2000-10-01
first Monday of Month: 2000-10-02
last day of Month: 2000-10-31
first day of next Month: 2000-11-01
first day of next Year: 2001-01-01
first day of Year: 2000-01-01

```

## 自定义调整器

您还可以创建自定义调整器。为此，您需要创建一个实现了`TemporalAdjuster`接口并带有[`adjustInto(Temporal)`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAdjuster.html#adjustInto-java.time.temporal.Temporal-)方法的类。来自``NextPayday``示例的`PaydayAdjuster`类是一个自定义调整器。`PaydayAdjuster`评估传入的日期并返回下一个发薪日，假设发薪日每月发生两次：在每月的 15 日和最后一天。如果计算出的日期落在周末，则使用前一个星期五。假定当前日历年。

```java
/**
 * The adjustInto method accepts a Temporal instance 
 * and returns an adjusted LocalDate. If the passed in
 * parameter is not a LocalDate, then a DateTimeException is thrown.
 */
public Temporal adjustInto(Temporal input) {
    LocalDate date = LocalDate.from(input);
    int day;
    if (date.getDayOfMonth() < 15) {
        day = 15;
    } else {
        day = date.with(TemporalAdjusters.lastDayOfMonth()).getDayOfMonth();
    }
    date = date.withDayOfMonth(day);
    if (date.getDayOfWeek() == DayOfWeek.SATURDAY ||
        date.getDayOfWeek() == DayOfWeek.SUNDAY) {
        date = date.with(TemporalAdjusters.previous(DayOfWeek.FRIDAY));
    }

    return input.with(date);
}

```

调整器的调用方式与预定义调整器相同，使用`with`方法。以下代码行来自`NextPayday`示例：

```java
LocalDate nextPayday = date.with(new PaydayAdjuster());

```

在 2013 年，6 月 15 日和 6 月 30 日都落在周末。使用 2013 年 6 月 3 日和 6 月 18 日的相应日期运行`NextPayday`示例，得到以下结果：

```java
Given the date:  2013 Jun 3
the next payday: 2013 Jun 14

Given the date:  2013 Jun 18
the next payday: 2013 Jun 28

```
