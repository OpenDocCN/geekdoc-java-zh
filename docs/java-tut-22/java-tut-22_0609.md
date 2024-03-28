# 执行与 Locale 无关的比较

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/locale.html`](https://docs.oracle.com/javase/tutorial/i18n/text/locale.html)

排序规则定义字符串的排序顺序。这些规则因区域而异，因为各种自然语言对单词的排序方式不同。您可以使用`Collator`类提供的预定义排序规则以与区域设置无关的方式对字符串进行排序。

要实例化`Collator`类，请调用`getInstance`方法。通常，您为默认的`Locale`创建一个`Collator`，如下例所示：

```java
Collator myDefaultCollator = Collator.getInstance();

```

创建`Collator`时，您还可以指定特定的`Locale`，如下所示：

```java
Collator myFrenchCollator = Collator.getInstance(Locale.FRENCH);

```

`getInstance`方法返回一个`RuleBasedCollator`，它是`Collator`的具体子类。`RuleBasedCollator`包含一组规则，这些规则确定您指定的区域设置的字符串排序顺序。这些规则对于每个区域设置都是预定义的。由于规则封装在`RuleBasedCollator`中，您的程序不需要特殊的例程来处理排序规则随语言变化的方式。

您调用`Collator.compare`方法执行与 Locale 无关的字符串比较。当第一个字符串参数小于、等于或大于第二个字符串参数时，`compare`方法返回小于零、等于零或大于零的整数。以下表格包含一些对`Collator.compare`的示例调用：

| 示例 | 返回值 | 解释 |
| --- | --- | --- |
| `myCollator.compare("abc", "def")` | `-1` | `"abc"`小于"def" |
| `myCollator.compare("rtf", "rtf")` | `0` | 两个字符串相等 |
| `myCollator.compare("xyz", "abc")` | `1` | "xyz"大于"abc" |

在执行排序操作时，您使用`compare`方法。名为`CollatorDemo`的示例程序使用`compare`方法对英语和法语单词数组进行排序。该程序展示了使用两个不同的排序器对相同单词列表进行排序时可能发生的情况：

```java
Collator fr_FRCollator = Collator.getInstance(new Locale("fr","FR"));
Collator en_USCollator = Collator.getInstance(new Locale("en","US"));

```

排序方法称为`sortStrings`，可以与任何`Collator`一起使用。请注意，`sortStrings`方法调用`compare`方法：

```java
public static void sortStrings(Collator collator, String[] words) {
    String tmp;
    for (int i = 0; i < words.length; i++) {
        for (int j = i + 1; j < words.length; j++) { 
            if (collator.compare(words[i], words[j]) > 0) {
                tmp = words[i];
                words[i] = words[j];
                words[j] = tmp;
            }
        }
    }
}

```

英语`Collator`按以下方式对单词进行排序：

```java
peach
péché
pêche
sin

```

根据法语语言的排序规则，前述列表的顺序是错误的。在法语中，"péché"应该在排序列表中跟在"pêche"之后。法语`Collator`正确地对单词数组进行排序，如下所示：

```java
peach
pêche
péché
sin

```
