# Matcher 类的方法

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/matcher.html`](https://docs.oracle.com/javase/tutorial/essential/regex/matcher.html)

本节描述了`Matcher`类的一些额外有用的方法。为方便起见，下面列出的方法根据功能进行了分组。

## 索引方法

*索引方法*提供了有用的索引值，精确显示匹配在输入字符串中的位置：

+   [`public int start()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#start--): 返回先前匹配的起始索引。

+   [`public int start(int group)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#start-int-): 返回在先前匹配操作期间由给定组捕获的子序列的起始索引。

+   [`public int end()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#end--): 返回匹配的最后一个字符后的偏移量。

+   [`public int end(int group)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#end-int-): 返回在先前匹配操作期间由给定组捕获的子序列的最后一个字符后的偏移量。

## 学习方法

*学习方法*审查输入字符串并返回一个布尔值，指示模式是否被找到。

+   [`public boolean lookingAt()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#lookingAt--): 尝试从区域的开头开始匹配输入序列与模式。

+   [`public boolean find()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#find--): 尝试查找与模式匹配的输入序列的下一个子序列。

+   [`public boolean find(int start)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#find-int-): 重置此匹配器，然后尝试查找从指定索引开始的输入序列的下一个与模式匹配的子序列。

+   [`public boolean matches()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#matches--): 尝试将整个区域与模式匹配。

## 替换方法

*替换方法*是用于替换输入字符串中文本的有用方法。

+   [`public Matcher appendReplacement(StringBuffer sb, String replacement)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#appendReplacement-java.lang.StringBuffer-java.lang.String-): 实现了一个非终端追加和替换步骤。

+   [`public StringBuffer appendTail(StringBuffer sb)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#appendTail-java.lang.StringBuffer-): 实现了一个终端追加和替换步骤。

+   [`public String replaceAll(String replacement)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#replaceAll-java.lang.String-)：用给定替换字符串替换与模式匹配的输入序列的每个子序列。

+   [`public String replaceFirst(String replacement)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#replaceFirst-java.lang.String-)：用给定的替换字符串替换与模式匹配的输入序列的第一个子序列。

+   [`public static String quoteReplacement(String s)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#quoteReplacement-java.lang.String-)：为指定的`String`返回一个字面替换`String`。此方法生成一个将作为`Matcher`类的`appendReplacement`方法中的字面替换`s`的`String`。生成的`String`将匹配将`s`视为字面序列处理的字符序列。反斜杠（`'\'`）和美元符号（`'$'`）将不被赋予特殊含义。

## 使用 `start` 和 `end` 方法

这里有一个例子，`MatcherDemo.java`，它计算输入字符串中单词"dog"出现的次数。

```java

import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class MatcherDemo {

    private static final String REGEX =
        "\\bdog\\b";
    private static final String INPUT =
        "dog dog dog doggie dogg";

    public static void main(String[] args) {
       Pattern p = Pattern.compile(REGEX);
       //  get a matcher object
       Matcher m = p.matcher(INPUT);
       int count = 0;
       while(m.find()) {
           count++;
           System.out.println("Match number "
                              + count);
           System.out.println("start(): "
                              + m.start());
           System.out.println("end(): "
                              + m.end());
      }
   }
}

```

```java
OUTPUT:

Match number 1
start(): 0
end(): 3
Match number 2
start(): 4
end(): 7
Match number 3
start(): 8
end(): 11

```

您可以看到，此示例使用单词边界来确保字母`"d" "o" "g"`不仅仅是更长单词的子字符串。它还提供了有关匹配发生在输入字符串的哪个位置的一些有用信息。`start` 方法返回在先前匹配操作期间由给定组捕获的子序列的起始索引，而 `end` 返回匹配的最后一个字符的索引加一。

## 使用 `matches` 和 `lookingAt` 方法

`matches` 和 `lookingAt` 方法都尝试将输入序列与模式进行匹配。然而，不同之处在于 `matches` 要求整个输入序列匹配，而 `lookingAt` 则不需要。这两种方法始终从输入字符串的开头开始。以下是完整代码，`MatchesLooking.java`：

```java

import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class MatchesLooking {

    private static final String REGEX = "foo";
    private static final String INPUT =
        "fooooooooooooooooo";
    private static Pattern pattern;
    private static Matcher matcher;

    public static void main(String[] args) {

        // Initialize
        pattern = Pattern.compile(REGEX);
        matcher = pattern.matcher(INPUT);

        System.out.println("Current REGEX is: "
                           + REGEX);
        System.out.println("Current INPUT is: "
                           + INPUT);

        System.out.println("lookingAt(): "
            + matcher.lookingAt());
        System.out.println("matches(): "
            + matcher.matches());
    }
}

```

```java
Current REGEX is: foo
Current INPUT is: fooooooooooooooooo
lookingAt(): true
matches(): false

```

## 使用 `replaceFirst(String)` 和 `replaceAll(String)`

`replaceFirst` 和 `replaceAll` 方法替换与给定正则表达式匹配的文本。正如它们的名称所示，`replaceFirst` 替换第一次出现，而 `replaceAll` 替换所有出现。以下是 `ReplaceDemo.java` 代码：

```java

import java.util.regex.Pattern; 
import java.util.regex.Matcher;

public class ReplaceDemo {

    private static String REGEX = "dog";
    private static String INPUT =
        "The dog says meow. All dogs say meow.";
    private static String REPLACE = "cat";

    public static void main(String[] args) {
        Pattern p = Pattern.compile(REGEX);
        // get a matcher object
        Matcher m = p.matcher(INPUT);
        INPUT = m.replaceAll(REPLACE);
        System.out.println(INPUT);
    }
}

```

```java
OUTPUT: The cat says meow. All cats say meow.

```

在这个第一个版本中，所有 `dog` 的出现都被替换为 `cat`。但为什么要停在这里呢？与其仅替换简单的文字`dog`，不如替换与*任何*正则表达式匹配的文本。该方法的 API 表明，“给定正则表达式`a*b`，输入`aabfooaabfooabfoob`，替换字符串为`-`，对该表达式的匹配器调用此方法将产生字符串`-foo-foo-foo-`。”

以下是 `ReplaceDemo2.java` 代码：

```java

import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class ReplaceDemo2 {

    private static String REGEX = "a*b";
    private static String INPUT =
        "aabfooaabfooabfoob";
    private static String REPLACE = "-";

    public static void main(String[] args) {
        Pattern p = Pattern.compile(REGEX);
        // get a matcher object
        Matcher m = p.matcher(INPUT);
        INPUT = m.replaceAll(REPLACE);
        System.out.println(INPUT);
    }
}

```

```java
OUTPUT: -foo-foo-foo-

```

要仅替换模式的第一次出现，只需调用 `replaceFirst` 而不是 `replaceAll`。它接受相同的参数。

## 使用 `appendReplacement(StringBuffer,String)` 和 `appendTail(StringBuffer)`

`Matcher` 类还提供了 `appendReplacement` 和 `appendTail` 方法用于文本替换。下面的示例，`RegexDemo.java` ，使用这两种方法实现与 `replaceAll` 相同的效果。

```java

import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class RegexDemo {

    private static String REGEX = "a*b";
    private static String INPUT = "aabfooaabfooabfoob";
    private static String REPLACE = "-";

    public static void main(String[] args) {
        Pattern p = Pattern.compile(REGEX);
        Matcher m = p.matcher(INPUT); // get a matcher object
        StringBuffer sb = new StringBuffer();
        while(m.find()){
            m.appendReplacement(sb,REPLACE);
        }
        m.appendTail(sb);
        System.out.println(sb.toString());
    }
}

```

```java
OUTPUT: -foo-foo-foo- 

```

## `java.lang.String` 中的 `Matcher` 方法等效

为方便起见，`String` 类也模仿了一些 `Matcher` 方法：

+   [`public String replaceFirst(String regex, String replacement)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#replaceFirst-java.lang.String-java.lang.String-)：用给定的替换内容替换此字符串匹配给定正则表达式的第一个子字符串。形式为 `*str*.replaceFirst(*regex*, *repl*)` 的此方法调用产生的结果与表达式 `Pattern.compile(*regex*).matcher(*str*).replaceFirst(*repl*)` 完全相同。

+   [`public String replaceAll(String regex, String replacement)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#replaceAll-java.lang.String-java.lang.String-)：用给定的替换内容替换此字符串中与给定正则表达式匹配的每个子字符串。形式为 `*str*.replaceAll(*regex*, *repl*)` 的此方法调用产生的结果与表达式 `Pattern.compile(*regex*).matcher(*str*).replaceAll(*repl*)` 完全相同。
