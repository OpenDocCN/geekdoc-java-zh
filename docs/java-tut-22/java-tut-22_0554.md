# 日期时间设计原则

> 原文：[`docs.oracle.com/javase/tutorial/datetime/overview/design.html`](https://docs.oracle.com/javase/tutorial/datetime/overview/design.html)

日期时间 API 是根据几个设计原则开发的。

## 清晰

API 中的方法被明确定义，其行为清晰可预期。例如，使用`null`参数值调用日期时间方法通常会触发`NullPointerException`。

## 流畅

日期时间 API 提供了流畅的接口，使代码易于阅读。因为大多数方法不允许带有`null`值的参数，并且不返回`null`值，方法调用可以链接在一起，结果代码可以快速理解。例如：

```java
LocalDate today = LocalDate.now();
LocalDate payday = today.with(TemporalAdjusters.lastDayOfMonth()).minusDays(2);

```

## 不可变

日期时间 API 中的大多数类创建的对象是不可变的，这意味着在对象创建后，它是不能被修改的。要修改不可变对象的值，必须构建一个修改后的原始副本作为新对象。这也意味着日期时间 API 在定义上是线程安全的。这影响了 API，大多数用于创建日期或时间对象的方法都以`of`、`from`或`with`为前缀，而不是构造函数，并且没有`set`方法。例如：

```java
LocalDate dateOfBirth = LocalDate.of(2012, Month.MAY, 14);
LocalDate firstBirthday = dateOfBirth.plusYears(1);

```

## 可扩展

日期时间 API 在尽可能的地方是可扩展的。例如，您可以定义自己的时间调整器和查询，或构建自己的日历系统。
