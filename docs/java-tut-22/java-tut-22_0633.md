# 教程：国际化的服务提供者

> 原文：[`docs.oracle.com/javase/tutorial/i18n/serviceproviders/index.html`](https://docs.oracle.com/javase/tutorial/i18n/serviceproviders/index.html)

国际化的服务提供者使得可以插入区域相关的数据和服务。由于可以插入区域相关的数据和服务，第三方能够提供`java.text`和`java.util`包中大多数区域敏感类的实现。

服务是一组提供对特定应用程序功能或特性访问的编程接口和类。服务提供者接口（SPI）是服务定义的一组公共接口和抽象类。服务提供者实现了 SPI。服务提供者使您能够创建可扩展的应用程序，可以在不修改其原始代码库的情况下进行扩展。您可以通过新的插件或模块增强其功能。有关服务提供者和可扩展应用程序的更多信息，请参阅创建可扩展应用程序。

您可以使用国际化的服务提供者来提供以下区域敏感类的自定义实现：

+   [`BreakIterator`](https://docs.oracle.com/javase/8/docs/api/java/text/BreakIterator.html) 对象

+   [`Collator`](https://docs.oracle.com/javase/8/docs/api/java/text/Collator.html) 对象

+   [`Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html) 类的语言代码、国家代码和变体名称

+   时区名称

+   货币符号

+   [`DateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormat.html) 对象

+   [`DateFormatSymbols`](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormatSymbols.html) 对象

+   [`NumberFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/NumberFormat.html) 对象

+   [`DecimalFormatSymbols`](https://docs.oracle.com/javase/8/docs/api/java/text/DecimalFormatSymbols.html) 对象

相应的 SPI 包含在`java.text.spi`和`java.util.spi`包中：

| `java.util.spi` | `java.text.spi` |
| --- | --- |

|

+   [`CurrencyNameProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/CurrencyNameProvider.html)

+   [`LocaleServiceProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/LocaleServiceProvider.html)

+   [`TimeZoneNameProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/TimeZoneNameProvider.html)

|

+   [`BreakIteratorProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/BreakIteratorProvider.html)

+   [`CollatorProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/CollatorProvider.html)

+   [`DateFormatProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/DateFormatProvider.html)

+   [`DateFormatSymbolsProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/DateFormatSymbolsProvider.html)

+   [`DecimalFormatSymbolsProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/DecimalFormatSymbolsProvider.html)

+   [`NumberFormatProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/NumberFormatProvider.html)

|

例如，如果您想为新的区域设置提供一个`NumberFormat`对象，请实现`java.text.spi.NumberFormatProvider`类并实现以下方法：

+   `getCurrencyInstance(Locale locale)`

+   `getIntegerInstance(Locale locale)`

+   `getNumberInstance(Locale locale)`

+   `getPercentInstance(Locale locale)`

```java
Locale loc = new Locale("da", "DK");
NumberFormat nf = NumberFormatProvider.getNumberInstance(loc);

```

这些方法首先检查 Java 运行时环境是否支持请求的区域设置；如果支持，则使用该支持。否则，方法会调用已安装提供者的适当接口的`getAvailableLocales`方法，以找到支持请求的区域设置的提供者。

要深入了解如何使用国际化服务提供者的示例，请参阅将自定义资源包作为扩展安装。本节展示了如何实现[`ResourceBundleControlProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/ResourceBundleControlProvider.html)接口，使您能够在不对应用程序源代码进行任何其他更改的情况下使用任何自定义`ResourceBundle.Control`类。
