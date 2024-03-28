# Period 和 Duration

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/period.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/period.html)

当编写代码来指定一段时间时，请使用最符合您需求的类或方法：[`Duration`](https://docs.oracle.com/javase/8/docs/api/java/time/Duration.html)类、[`Period`](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html)类或[`ChronoUnit.between`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoUnit.html#between-java.time.temporal.Temporal-java.time.temporal.Temporal-)方法。`Duration`使用基于时间的值（秒、纳秒）来衡量一段时间。`Period`使用基于日期的值（年、月、日）。

* * *

**注意：** 一天的`Duration`*确切地*为 24 小时。将一天的`Period`添加到`ZonedDateTime`中时，根据时区可能会有所变化。例如，如果发生在夏令时的第一天或最后一天。

* * *

## Duration

一个`Duration`在衡量机器时间的情况下最为适用，比如使用`Instant`对象的代码。`Duration`对象以秒或纳秒为单位进行衡量，不使用年、月和日等基于日期的构造，尽管该类提供了将时间转换为天、小时和分钟的方法。`Duration`可以具有负值，如果创建时结束点早于起始点。

以下代码计算两个瞬间之间的持续时间（以纳秒为单位）：

```java
Instant t1, t2;
...
long ns = Duration.between(t1, t2).toNanos();

```

以下代码将 10 秒添加到一个`Instant`中：

```java
Instant start;
...
Duration gap = Duration.ofSeconds(10);
Instant later = start.plus(gap);

```

一个`Duration`与时间线无关，即它不跟踪时区或夏令时。将相当于 1 天的`Duration`添加到`ZonedDateTime`中，结果将确切地添加 24 小时，而不考虑夏令时或其他可能导致的时间差异。

## ChronoUnit

讨论 Temporal Package 中的`ChronoUnit`枚举定义了用于衡量时间的单位。当您想要以单个时间单位（如天或秒）衡量一段时间时，`ChronoUnit.between`方法非常有用。`between`方法适用于所有基于时间的对象，但仅以单个单位返回时间量。以下代码计算两个时间戳之间的间隔，以毫秒为单位：

```java
import java.time.Instant;
import java.time.temporal.Temporal;
import java.time.temporal.ChronoUnit;

Instant previous, current, gap;
...
current = Instant.now();
if (previous != null) {
    gap = ChronoUnit.MILLIS.between(previous,current);
}
...

```

## Period

要使用基于日期的值（年、月、日）来定义一段时间，请使用[`Period`](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html)类。`Period`类提供了各种`get`方法，比如[`getMonths`](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html#getMonths--)、[`getDays`](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html#getDays--)和[`getYears`](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html#getYears--)，以便您可以从期间中提取时间量。

所有三个单位（月、日和年）一起表示的总时间段。要以单个时间单位（如天）表示测量的时间量，可以使用`ChronoUnit.between`方法。

以下代码报告了你的年龄，假设你出生于 1960 年 1 月 1 日。使用`Period`类来确定年、月和日的时间。使用`ChronoUnit.between`方法确定相同的时间段，以总天数显示在括号中：

```java
LocalDate today = LocalDate.now();
LocalDate birthday = LocalDate.of(1960, Month.JANUARY, 1);

Period p = Period.between(birthday, today);
long p2 = ChronoUnit.DAYS.between(birthday, today);
System.out.println("You are " + p.getYears() + " years, " + p.getMonths() +
                   " months, and " + p.getDays() +
                   " days old. (" + p2 + " days total)");

```

该代码生成类似以下的输出：

```java
You are 53 years, 4 months, and 29 days old. (19508 days total)

```

要计算距离你下一个生日还有多长时间，可以使用来自``Birthday``示例的以下代码。使用`Period`类来确定月和日的值。`ChronoUnit.between`方法返回总天数的值，并显示在括号中。

```java
LocalDate birthday = LocalDate.of(1960, Month.JANUARY, 1);

LocalDate nextBDay = birthday.withYear(today.getYear());

//If your birthday has occurred this year already, add 1 to the year.
if (nextBDay.isBefore(today) || nextBDay.isEqual(today)) {
    nextBDay = nextBDay.plusYears(1);
}

Period p = Period.between(today, nextBDay);
long p2 = ChronoUnit.DAYS.between(today, nextBDay);
System.out.println("There are " + p.getMonths() + " months, and " +
                   p.getDays() + " days until your next birthday. (" +
                   p2 + " total)");

```

该代码生成类似以下的输出：

```java
There are 7 months, and 2 days until your next birthday. (216 total)

```

这些计算没有考虑时区差异。例如，如果你出生在澳大利亚，但目前居住在班加罗尔，这会稍微影响你确切年龄的计算。在这种情况下，可以结合使用`Period`和`ZonedDateTime`类。当你将`Period`添加到`ZonedDateTime`时，会考虑时差。
