# 使用预定义格式

> 原文：[`docs.oracle.com/javase/tutorial/i18n/format/dateFormat.html`](https://docs.oracle.com/javase/tutorial/i18n/format/dateFormat.html)

* * *

**版本说明：** 本日期和时间部分使用了`java.util`包中的日期和时间 API。在 JDK 8 发布的`java.time`API 中，提供了一个全面的日期和时间模型，相比`java.util`类有显著的改进。`java.time`API 在日期时间教程中有详细描述。旧日期时间代码页面可能会引起特别关注。

* * *

[`DateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormat.html)类允许您以区域敏感的方式使用预定义样式格式化日期和时间。接下来的部分演示了如何使用名为`DateFormatDemo.java`的程序与`DateFormat`类一起使用。

## 日期

使用`DateFormat`类格式化日期是一个两步过程。首先，使用`getDateInstance`方法创建格式化器。其次，调用`format`方法，返回包含格式化日期的`String`。以下示例通过调用这两个方法格式化今天的日期：

```java
Date today;
String dateOut;
DateFormat dateFormatter;

dateFormatter = DateFormat.getDateInstance(DateFormat.DEFAULT, currentLocale);
today = new Date();
dateOut = dateFormatter.format(today);

System.out.println(dateOut + " " + currentLocale.toString());

```

以下是此代码生成的输出。请注意日期的格式会随着`Locale`的不同而变化。由于`DateFormat`是区域敏感的，它会为每个`Locale`处理格式化细节。

```java
30 juin 2009     fr_FR
30.06.2009       de_DE
Jun 30, 2009     en_US

```

前面的代码示例指定了`默认`格式样式。`默认`样式只是`DateFormat`类提供的预定义格式样式之一，如下所示：

+   默认

+   短

+   中等

+   长

+   完整

以下表显示了美国和法国区域的每种样式的日期格式：

示例日期格式

| 样式 | 美国区域 | 法国区域 |
| --- | --- | --- |
| `默认` | 2009 年 6 月 30 日 | 2009 年 6 月 30 日 |
| `短` | 6/30/09 | 30/06/09 |
| `中等` | 2009 年 6 月 30 日 | 2009 年 6 月 30 日 |
| `长` | 2009 年 6 月 30 日 | 2009 年 6 月 30 日 |
| `完整` | 2009 年 6 月 30 日星期二 | 2009 年 6 月 30 日星期二 |

## 时间

`Date`对象代表日期和时间。使用`DateFormat`类格式化时间与格式化日期类似，只是你需要使用`getTimeInstance`方法创建格式化器，如下所示：

```java
DateFormat timeFormatter =
    DateFormat.getTimeInstance(DateFormat.DEFAULT, currentLocale);

```

下表显示了美国和德国区域的各种预定义格式样式：

示例时间格式

| 样式 | 美国区域 | 德国区域 |
| --- | --- | --- |
| `默认` | 上午 7:03:47 | 7:03:47 |
| `短` | 上午 7:03 | 07:03 |
| `中等` | 上午 7:03:47 | 07:03:07 |
| `长` | 上午 7:03:47 PDT | 07:03:45 PDT |
| `完整` | 上午 7:03:47 PDT | 下午 7.03 PDT |

## 日期和时间都包括

要在同一`String`中显示日期和时间，使用`getDateTimeInstance`方法创建格式化器。第一个参数是日期样式，第二个是时间样式。第三个参数是`Locale`。以下是一个快速示例：

```java
DateFormat formatter = DateFormat.getDateTimeInstance(
                           DateFormat.LONG, 
                           DateFormat.LONG, 
                           currentLocale);

```

以下表显示了美国和法国区域的日期和时间格式样式：

日期和时间格式示例

| 样式 | 美国区域设置 | 法国区域设置 |
| --- | --- | --- |
| `DEFAULT` | Jun 30, 2009 7:03:47 AM | 30 juin 2009 07:03:47 |
| `SHORT` | 6/30/09 7:03 AM | 30/06/09 07:03 |
| `MEDIUM` | Jun 30, 2009 7:03:47 AM | 30 juin 2009 07:03:47 |
| `LONG` | June 30, 2009 7:03:47 AM PDT | 30 juin 2009 07:03:47 PDT |
| `FULL` | Tuesday, June 30, 2009 7:03:47 AM PDT | mardi 30 juin 2009 07 h 03 PDT |
