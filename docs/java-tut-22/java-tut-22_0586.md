# 语言标签过滤和查找

> 原文：[`docs.oracle.com/javase/tutorial/i18n/locale/matching.html`](https://docs.oracle.com/javase/tutorial/i18n/locale/matching.html)

Java 编程语言包含对语言标签、语言标签过滤和语言标签查找的国际化支持。这些功能由[IETF BCP 47](http://tools.ietf.org/html/bcp47)规定，其中包括[RFC 5646“用于标识语言的标签”](http://tools.ietf.org/html/rfc5646)和[RFC 4647“语言标签的匹配”](http://tools.ietf.org/html/rfc4647)。本课程描述了 JDK 中提供这种支持的方式。

## 什么是语言标签？

语言标签是专门格式化的字符串，提供有关特定语言的信息。语言标签可能是简单的（如“en”表示英语）、复杂的（如“zh-cmn-Hans-CN”表示中国普通话，简体字，用于中国）或介于两者之间的（如“sr-Latn”，表示使用拉丁文写的塞尔维亚语）。语言标签由连字符分隔的“子标签”组成；这个术语在整个 API 文档中使用。

[`java.util.Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)类提供对语言标签的支持。`Locale`包含几个不同的字段：语言（如“en”表示英语，或“ja”表示日语）、脚本（如“Latn”表示拉丁文或“Cyrl”表示西里尔文）、国家（如“US”表示美国或“FR”表示法国）、变体（指示区域的某些变体）和扩展（提供单字符键到`String`值的映射，表示除语言标识之外的扩展）。要从语言标签`String`创建`Locale`对象，请调用[`Locale.forLanguageTag(String)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#forLanguageTag-java.lang.String-)，将语言标签作为唯一参数传入。这样做会创建并返回一个新的`Locale`对象，供应用程序使用。

示例 1：

```java
package languagetagdemo;

import java.util.Locale;

public class LanguageTagDemo {
     public static void main(String[] args) {
         Locale l = Locale.forLanguageTag("en-US");
     }
}

```

请注意，Locale API 只要求您的语言标签在语法上是格式良好的。它不执行任何额外的验证（例如检查标签是否在 IANA 语言子标签注册表中注册）。

## 什么是语言范围？

语言范围（由类[`java.util.Locale.LanguageRange`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.LanguageRange.html)表示）标识具有特定属性的语言标签集。语言范围分为基本和扩展两类，类似于语言标签，由连字符分隔的子标签组成。基本语言范围的示例包括“en”（英语）、“ja-JP”（日语，日本）和“*”（特殊语言范围，匹配任何语言标签）。扩展语言范围的示例包括“*-CH”（任何语言，瑞士）、“es-*”（西班牙语，任何地区）和“zh-Hant-*”（繁体中文，任何地区）。

此外，语言范围可以存储在语言优先级列表中，这使用户可以在加权列表中优先考虑他们的语言偏好。语言优先级列表通过将`LanguageRange`对象放入[`java.util.List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)中来表示，然后可以将其传递给接受`List`的`LanguageRange`对象的`Locale`方法。

## 创建语言范围

[`Locale.LanguageRange`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.LanguageRange.html)类提供了两种不同的构造函数来创建语言范围：

+   `public Locale.LanguageRange(String range)`

+   `public Locale.LanguageRange(String range, double weight)`

它们之间唯一的区别是第二个版本允许指定权重；如果将范围放入语言优先级列表中，则将考虑此权重。

`Locale.LanguageRange`还指定了一些常量，用于与这些构造函数一起使用：

+   `public static final double MAX_WEIGHT`

+   `public static final double MIN_WEIGHT`

`MAX_WEIGHT`常量保存值 1.0，表示它非常适合用户。`MIN_WEIGHT`常量保存值 0.0，表示它不适合。

示例 2：

```java
package languagetagdemo;

import java.util.Locale;

public class LanguageTagDemo {

     public static void main(String[] args) {
         // Create Locale
         Locale l = Locale.forLanguageTag("en-US");

         // Define Some LanguageRange Objects
         Locale.LanguageRange range1 = new Locale.LanguageRange("en-US",Locale.LanguageRange.MAX_WEIGHT);
         Locale.LanguageRange range2 = new Locale.LanguageRange("en-GB*",0.5);
         Locale.LanguageRange range3 = new Locale.LanguageRange("fr-FR",Locale.LanguageRange.MIN_WEIGHT);
     }
}

```

示例 2 创建了三种语言范围：英语（美国）、英语（英国）和法语（法国）。这些范围被加权以表达用户的偏好，从最喜欢到最不喜欢的顺序。

## 创建语言优先级列表

您可以使用[`LanguageRange.parse(String)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.LanguageRange.html#parse-java.lang.String-)方法从语言范围列表创建语言优先级列表。此方法接受一个逗号分隔的语言范围列表，在给定范围中对每个语言范围执行语法检查，然后返回新创建的语言优先级列表。

有关“ranges”参数所需格式的详细信息，请参阅此方法的 API 规范。

示例 3：

```java
package languagetagdemo;

import java.util.Locale;

import java.util.List;

public class LanguageTagDemo {

    public static void main(String[] args) {

        // Create Locale

        Locale l = Locale.forLanguageTag("en-US");

        // Create a Language Priority List

        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";

        List<Locale.LanguageRange> languageRanges = Locale.LanguageRange.parse(ranges)

    }
}

```

示例 3 创建了与示例 2 相同的三种语言范围，但将它们存储在一个`String`对象中，该对象被传递给`parse(String)`方法。返回的`LanguageRange`对象的`List`是语言优先级列表。

## 过滤语言标签

语言标签过滤是将一组语言标签与用户的语言优先级列表进行匹配的过程。过滤的结果将是所有匹配结果的完整列表。`Locale`类定义了两个过滤方法，它们返回一个`Locale`对象的列表。它们的签名如下：

+   [`public static List<Locale> filter (List<Locale.LanguageRange> priorityList, Collection<Locale> locales)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#filter-java.util.List-java.util.Collection-)

+   [`public static List<Locale> filter (List<Locale.LanguageRange> priorityList, Collection<Locale> locales, Locale.FilteringMode mode)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#filter-java.util.List-java.util.Collection-java.util.Locale.FilteringMode-)

在这两种方法中，第一个参数指定了用户的语言优先级列表，如前一节所述。

第二个参数指定要匹配的`Locale`对象的`Collection`。匹配本身将根据 RFC 4647 指定的规则进行。

第三个参数（如果提供）指定要使用的“过滤模式”。[`Locale.FilteringMode`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.FilteringMode.html)枚举提供了许多不同的值可供选择，例如`AUTOSELECT_FILTERING`（用于基本语言范围过滤）或`EXTENDED_FILTERING`（用于扩展语言范围过滤）。

示例 4 演示了语言标记过滤。

示例 4：

```java
package languagetagdemo;

import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;

public class LanguageTagDemo {

    public static void main(String[] args) {

        // Create a collection of Locale objects to filter
        Collection<Locale> locales = new ArrayList<>();
        locales.add(Locale.forLanguageTag("en-GB"));
        locales.add(Locale.forLanguageTag("ja"));
        locales.add(Locale.forLanguageTag("zh-cmn-Hans-CN"));
        locales.add(Locale.forLanguageTag("en-US"));

        // Express the user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List<Locale.LanguageRange> languageRanges = Locale.LanguageRange.parse(ranges);

        // Now filter the Locale objects, returning any matches
        List<Locale> results = Locale.filter(languageRanges,locales);

        // Print out the matches
        for(Locale l : results){
        System.out.println(l.toString());
        }
    }
}

```

该程序的输出是：

en_US

en_GB

返回的列表根据用户的语言优先级列表中指定的权重进行排序。

`Locale`类还定义了`filterTags`方法，用于将语言标记过滤为`String`对象。

方法签名如下：

+   [`public static List<String> filterTags (List<Locale.LanguageRange> priorityList, Collection<String> tags)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#filterTags-java.util.List-java.util.Collection-)

+   [`public static List<String> filterTags (List<Locale.LanguageRange> priorityList, Collection<String> tags, Locale.FilteringMode mode)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#filterTags-java.util.List-java.util.Collection-java.util.Locale.FilteringMode-)

示例 5 提供了与示例 4 相同的搜索，但使用了`String`对象而不是`Locale`对象。

示例 5：

```java
package languagetagdemo;

import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;

public class LanguageTagDemo {

    public static void main(String[] args) {

        // Create a collection of String objects to match against
        Collection<String> tags = new ArrayList<>();
        tags.add("en-GB");
        tags.add("ja");
        tags.add("zh-cmn-Hans-CN");
        tags.add("en-US");

        // Express user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List<Locale.LanguageRange> languageRanges = Locale.LanguageRange.parse(ranges);

        // Now search the locales for the best match
        List<String> results = Locale.filterTags(languageRanges,tags);

        // Print out the matches
        for(String s : results){
            System.out.println(s);
        }
    }
} 

```

与以前一样，搜索将匹配并返回“en-US”和“en-GB”（按顺序）。

## 执行语言标记查找

与语言标记过滤相反，语言标记查找是将语言范围与语言标记集匹配并返回最佳匹配的语言标记的过程。RFC4647 规定：“查找从可用标记列表中产生最佳匹配用户偏好的单个结果，因此在需要单个项目的情况下很有用（并且只能返回一个项目）。例如，如果一个过程要将可读的错误消息插入协议头部，它可能会根据用户的语言优先级列表选择文本。由于该过程只能返回一个项目，因此必须选择一个项目并返回某个项目，即使内容的语言标记都不匹配用户提供的语言优先级列表。”

示例 6：

```java
package languagetagdemo;

import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;

public class LanguageTagDemo {

    public static void main(String[] args) {

        // Create a collection of Locale objects to search
        Collection<Locale> locales = new ArrayList<>();
        locales.add(Locale.forLanguageTag("en-GB"));
        locales.add(Locale.forLanguageTag("ja"));
        locales.add(Locale.forLanguageTag("zh-cmn-Hans-CN"));
        locales.add(Locale.forLanguageTag("en-US"));

        // Express the user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List<Locale.LanguageRange> languageRanges = Locale.LanguageRange.parse(ranges);

        // Find the BEST match, and return just one result
        Locale result = Locale.lookup(languageRanges,locales);
        System.out.println(result.toString());
    }
}

```

与过滤示例相比，在示例 6 中的查找演示返回最佳匹配的一个对象（在这种情况下是`en-US`）。为了完整起见，示例 7 展示了如何使用`String`对象执行相同的查找。

示例 7:

```java
package languagetagdemo;

import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;

public class LanguageTagDemo {

    public static void main(String[] args) {
        // Create a collection of String objects to match against
        Collection<String> tags = new ArrayList<>();
        tags.add("en-GB");
        tags.add("ja");
        tags.add("zh-cmn-Hans-CN");
        tags.add("en-US");

        // Express user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List<Locale.LanguageRange> languageRanges = Locale.LanguageRange.parse(ranges);

        // Find the BEST match, and return just one result
        String result = Locale.lookupTag(languageRanges, tags);
        System.out.println(result);
    }
} 

```

这个示例返回与用户的语言优先级列表最匹配的单个对象。
