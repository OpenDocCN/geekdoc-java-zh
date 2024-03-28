# 时钟

> 原文：[`docs.oracle.com/javase/tutorial/datetime/iso/clock.html`](https://docs.oracle.com/javase/tutorial/datetime/iso/clock.html)

大多数基于时间的对象提供一个无参数的`now()`方法，使用系统时钟和默认时区提供当前日期和时间。这些基于时间的对象还提供一个带有一个参数的`now(Clock)`方法，允许您传入一个替代的[`Clock`](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html)。

当前日期和时间取决于时区，对于全球化应用程序，需要一个`Clock`来确保日期/时间是使用正确的时区创建的。因此，虽然使用`Clock`类是可选的，但这个特性允许您测试您的代码是否适用于其他时区，或者通过使用一个固定的时钟，时间不会改变。

`Clock`类是抽象的，因此您不能创建它的实例。以下工厂方法对于测试很有用。

+   [`Clock.offset(Clock, Duration)`](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html#offset-java.time.Clock-java.time.Duration-) 返回一个按指定`Duration`偏移的时钟。

+   [`Clock.systemUTC()`](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html#systemUTC--) 返回代表格林威治/UTC 时区的时钟。

+   [`Clock.fixed(Instant, ZoneId)`](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html#fixed-java.time.Instant-java.time.ZoneId-) 总是返回相同的`Instant`。对于这个时钟，时间停滞不前。
