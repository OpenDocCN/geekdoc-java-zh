# 区域敏感服务 SPI

> 原文：[`docs.oracle.com/javase/tutorial/i18n/locale/services.html`](https://docs.oracle.com/javase/tutorial/i18n/locale/services.html)

此功能使得可以插入依赖于区域的数据和服务。通过这种方式，第三方能够提供 `java.text` 和 `java.util` 包中大多数区域敏感类的实现。

SPIs *(服务提供者接口)* 的实现基于由服务提供者实现的抽象类和 Java 接口。在运行时，Java 类加载机制用于动态定位和加载实现 SPI 的类。

您可以使用区域敏感的服务 SPI 来提供以下区域敏感的实现：

+   `BreakIterator` 对象

+   `Collator` 对象

+   `Locale` 类的语言代码、国家代码和变体名称

+   时区名称

+   货币符号

+   `DateFormat` 对象

+   `DateFormatSymbol` 对象

+   `NumberFormat` 对象

+   `DecimalFormatSymbols` 对象

相应的 SPI 包含在 `java.text.spi` 和 `java.util.spi` 包中：

| `java.util.spi` | `java.text.spi` |
| --- | --- |

|

+   `CurrencyNameProvider`

+   `LocaleServiceProvider`

+   `TimeZoneNameProvider`

+   `CalendarDataProvider`

|

+   `BreakIteratorProvider`

+   `CollatorProvider`

+   `DateFormatProvider`

+   `DateFormatSymbolsProvider`

+   `DecimalFormatSymbolsProvider`

+   `NumberFormatProvider`

|

例如，如果您想为新的区域提供一个 `NumberFormat` 对象，您必须实现 `java.text.spi.NumberFormatProvider` 类。您需要扩展此类并实现其方法：

+   `getCurrencyInstance(Locale locale)`

+   `getIntegerInstance(Locale locale)`

+   `getNumberInstance(Locale locale)`

+   `getPercentInstance(Locale locale)`

```java
Locale loc = new Locale("da", "DK");
NumberFormat nf = NumberFormatProvider.getNumberInstance(loc);

```

这些方法首先检查 Java 运行时环境是否支持请求的区域；如果支持，则使用该支持。否则，方法调用已安装的提供程序的适当接口的 `getAvailableLocales()` 方法，以找到支持请求的区域的提供程序。
