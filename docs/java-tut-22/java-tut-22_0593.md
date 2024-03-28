# 使用 ListResourceBundle

> 原文：[`docs.oracle.com/javase/tutorial/i18n/resbundle/list.html`](https://docs.oracle.com/javase/tutorial/i18n/resbundle/list.html)

本节演示了使用 `ListResourceBundle` 对象的示例程序 ``ListDemo``。接下来的文本解释了创建 `ListDemo` 程序所涉及的每个步骤，以及支持它的 `ListResourceBundle` 子类。

## 1\. 创建 ListResourceBundle 子类

`ListResourceBundle` 由类文件支持。因此，第一步是为每个支持的 `Locale` 创建一个类文件。在 `ListDemo` 程序中，`ListResourceBundle` 的基本名称是 `StatsBundle`。由于 `ListDemo` 支持三个 `Locale` 对象，因此需要以下三个类文件：

```java
StatsBundle_en_CA.class
StatsBundle_fr_FR.class
StatsBundle_ja_JP.class

```

为日本定义的 `StatsBundle` 类在接下来的源代码中定义。请注意，类名是通过将语言和国家代码附加到 `ListResourceBundle` 的基本名称构建的。在类内部，二维 `contents` 数组使用键值对进行初始化。键是每对中的第一个元素：`GDP`、`Population` 和 `Literacy`。键必须是 `String` 对象，并且在 `StatsBundle` 集合中的每个类中必须相同。值可以是任何类型的对象。在此示例中，值是两个 `Integer` 对象和一个 `Double` 对象。

```java
import java.util.*;
public class StatsBundle_ja_JP extends ListResourceBundle {
    public Object[][] getContents() {
        return contents;
    }

    private Object[][] contents = {
        { "GDP", new Integer(21300) },
        { "Population", new Integer(125449703) },
        { "Literacy", new Double(0.99) },
    };
}

```

## 2\. 指定 Locale

`ListDemo` 程序如下定义 `Locale` 对象：

```java
Locale[] supportedLocales = {
    new Locale("en", "CA"),
    new Locale("ja", "JP"),
    new Locale("fr", "FR")
};

```

每个 `Locale` 对象对应于一个 `StatsBundle` 类。例如，用 `ja` 和 `JP` 代码定义的日语 `Locale` 与 `StatsBundle_ja_JP.class` 匹配。

## 3\. 创建 ResourceBundle

要创建 `ListResourceBundle`，请调用 `getBundle` 方法。以下代码行指定了类的基本名称（`StatsBundle`）和 `Locale`：

```java
ResourceBundle stats = ResourceBundle.getBundle("StatsBundle", currentLocale);

```

`getBundle` 方法搜索以 `StatsBundle` 开头，后跟指定 `Locale` 的语言和国家代码的类。如果 `currentLocale` 是用 `ja` 和 `JP` 代码创建的，`getBundle` 将返回与类 `StatsBundle_ja_JP` 对应的 `ListResourceBundle`，例如。

## 4\. 获取本地化对象

现在程序有了适当 `Locale` 的 `ListResourceBundle`，它可以通过其键获取本地化对象。以下代码行通过使用 `Literacy` 键参数调用 `getObject` 来检索识字率。由于 `getObject` 返回一个对象，请将其转换为 `Double`：

```java
Double lit = (Double)stats.getObject("Literacy");

```

## 5\. 运行演示程序

`ListDemo` 程序打印了使用 `getBundle` 方法获取的数据：

```java
Locale = en_CA
GDP = 24400
Population = 28802671
Literacy = 0.97

Locale = ja_JP
GDP = 21300
Population = 125449703
Literacy = 0.99

Locale = fr_FR
GDP = 20200
Population = 58317450
Literacy = 0.99

```
