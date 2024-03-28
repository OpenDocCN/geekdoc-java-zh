# 时间查询

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/queries.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/queries.html)

[`TemporalQuery`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQuery.html)可用于从基于时间的对象中检索信息。

## 预定义查询

[`TemporalQueries`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQueries.html)类（注意是复数形式）提供了几个预定义查询，包括在应用程序无法识别基于时间的对象类型时有用的方法。与调整器一样，预定义查询被定义为静态方法，并设计用于与静态导入语句一起使用。

例如，[`precision`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQueries.html#precision--)查询返回特定基于时间的对象可以返回的最小`ChronoUnit`。以下示例在几种类型的基于时间的对象上使用了`precision`查询：

```java
TemporalQuery<TemporalUnit> query = TemporalQueries.precision();
System.out.printf("LocalDate precision is %s%n",
                  LocalDate.now().query(query));
System.out.printf("LocalDateTime precision is %s%n",
                  LocalDateTime.now().query(query));
System.out.printf("Year precision is %s%n",
                  Year.now().query(query));
System.out.printf("YearMonth precision is %s%n",
                  YearMonth.now().query(query));
System.out.printf("Instant precision is %s%n",
                  Instant.now().query(query));

```

输出如下所示：

```java
LocalDate precision is Days
LocalDateTime precision is Nanos
Year precision is Years
YearMonth precision is Months
Instant precision is Nanos

```

## 自定义查询

您还可以创建自定义查询。一种方法是创建一个实现了`TemporalQuery`接口的类，并使用[`queryFrom(TemporalAccessor)`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQuery.html#queryFrom-java.time.temporal.TemporalAccessor-)方法。``CheckDate``示例实现了两个自定义查询。第一个自定义查询可以在``FamilyVacations``类中找到，该类实现了[`TemporalQuery`](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQuery.html)接口。`queryFrom`方法将传入的日期与计划的假期日期进行比较，并在日期范围内返回`TRUE`。

```java
// Returns true if the passed-in date occurs during one of the
// family vacations. Because the query compares the month and day only,
// the check succeeds even if the Temporal types are not the same.
public Boolean queryFrom(TemporalAccessor date) {
    int month = date.get(ChronoField.MONTH_OF_YEAR);
    int day   = date.get(ChronoField.DAY_OF_MONTH);

    // Disneyland over Spring Break
    if ((month == Month.APRIL.getValue()) && ((day >= 3) && (day <= 8)))
        return Boolean.TRUE;

    // Smith family reunion on Lake Saugatuck
    if ((month == Month.AUGUST.getValue()) && ((day >= 8) && (day <= 14)))
        return Boolean.TRUE;

    return Boolean.FALSE;
}

```

第二个自定义查询在`FamilyBirthdays`类中实现。该类提供了一个`isFamilyBirthday`方法，用于将传入的日期与几个生日进行比较，如果匹配则返回`TRUE`。

```java
// Returns true if the passed-in date is the same as one of the
// family birthdays. Because the query compares the month and day only,
// the check succeeds even if the Temporal types are not the same.
public static Boolean isFamilyBirthday(TemporalAccessor date) {
    int month = date.get(ChronoField.MONTH_OF_YEAR);
    int day   = date.get(ChronoField.DAY_OF_MONTH);

    // Angie's birthday is on April 3.
    if ((month == Month.APRIL.getValue()) && (day == 3))
        return Boolean.TRUE;

    // Sue's birthday is on June 18.
    if ((month == Month.JUNE.getValue()) && (day == 18))
        return Boolean.TRUE;

    // Joe's birthday is on May 29.
    if ((month == Month.MAY.getValue()) && (day == 29))
        return Boolean.TRUE;

    return Boolean.FALSE;
}

```

`FamilyBirthday`类未实现`TemporalQuery`接口，可作为 lambda 表达式的一部分使用。来自`CheckDate`示例的以下代码展示了如何调用两个自定义查询。

```java
// Invoking the query without using a lambda expression.
Boolean isFamilyVacation = date.query(new FamilyVacations());

// Invoking the query using a lambda expression.
Boolean isFamilyBirthday = date.query(FamilyBirthdays::isFamilyBirthday);

if (isFamilyVacation.booleanValue() || isFamilyBirthday.booleanValue())
    System.out.printf("%s is an important date!%n", date);
else
    System.out.printf("%s is not an important date.%n", date);

```
