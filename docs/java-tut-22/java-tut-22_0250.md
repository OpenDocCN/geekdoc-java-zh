# 模式类的方法

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/pattern.html`](https://docs.oracle.com/javase/tutorial/essential/regex/pattern.html)

到目前为止，我们只使用测试工具来创建`Pattern`对象的最基本形式。本节探讨了一些高级技术，如使用标志创建模式和使用嵌入式标志表达式。它还探讨了一些我们尚未讨论的其他有用方法。

## 使用标志创建模式

`Pattern`类定义了一个替代的`compile`方法，接受一组影响模式匹配方式的标志。标志参数是一个位掩码，可以包括以下任何公共静态字段：

+   [`Pattern.CANON_EQ`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#CANON_EQ) 启用规范等价性。当指定此标志时，只有当两个字符的完整规范分解匹配时，它们才被视为匹配。例如，表达式`"a\u030A"`将在指定此标志时匹配字符串`"\u00E5"`。默认情况下，匹配不考虑规范等价性。指定此标志可能会带来性能损失。

+   [`Pattern.CASE_INSENSITIVE`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#CASE_INSENSITIVE) 启用不区分大小写的匹配。默认情况下，不区分大小写的匹配假定只有在 US-ASCII 字符集中的字符被匹配。通过与此标志一起指定 UNICODE_CASE 标志，可以启用 Unicode 感知的不区分大小写匹配。不区分大小写的匹配也可以通过嵌入式标志表达式`(?i)`启用。指定此标志可能会带来轻微的性能损失。

+   [`Pattern.COMMENTS`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#COMMENTS) 允许在模式中使用空格和注释。在此模式下，空格被忽略，并且以`#`开头的嵌入式注释被忽略直到行尾。注释模式也可以通过嵌入式标志表达式`(?x)`启用。

+   [`Pattern.DOTALL`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#DOTALL) 启用 dotall 模式。在 dotall 模式下，表达式`.`匹配任何字符，包括行终止符。默认情况下，此表达式不匹配行终止符。Dotall 模式也可以通过嵌入式标志表达式`(?s)`启用。（s 是"single-line"模式的缩写，在 Perl 中称为这种模式。）

+   [`Pattern.LITERAL`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#LITERAL) 启用模式的字面解析。当指定此标志时，指定模式的输入字符串被视为一系列字面字符。输入序列中的元字符或转义序列将不被赋予特殊含义。当与此标志一起使用时，`CASE_INSENSITIVE` 和 `UNICODE_CASE` 标志在匹配时保留其影响。其他标志变得多余。没有嵌入式标志字符用于启用字面解析。

+   [`Pattern.MULTILINE`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#MULTILINE) 启用多行模式。在多行模式下，表达式`^`和`$`分别在行终止符之后或之前匹配，或在输入序列的末尾。默认情况下，这些表达式仅在整个输入序列的开头和结尾匹配。多行模式也可以通过嵌入式标志表达式`(?m)`启用。

+   [`Pattern.UNICODE_CASE`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#UNICODE_CASE) 启用 Unicode 感知大小写折叠。当指定此标志时，大小写不敏感匹配（由`CASE_INSENSITIVE`标志启用）将按照 Unicode 标准一致的方式进行。默认情况下，大小写不敏感匹配假定只匹配 US-ASCII 字符集中的字符。Unicode 感知大小写折叠也可以通过嵌入式标志表达式`(?u)`启用。指定此标志可能会带来性能损失。

+   [`Pattern.UNIX_LINES`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#UNIX_LINES) 启用 UNIX 行模式。在此模式下，只有`'\n'`行终止符在`.`, `^`, 和`$`的行为中被识别。UNIX 行模式也可以通过嵌入式标志表达式`(?d)`启用。

在以下步骤中，我们将修改测试工具`RegexTestHarness.java`以创建一个具有大小写不敏感匹配的模式。

首先，修改代码以调用`compile`的另一个版本：

```java
Pattern pattern = 
Pattern.compile(console.readLine("%nEnter your regex: "),
Pattern.CASE_INSENSITIVE);

```

然后编译并运行测试工具以获得以下结果：

```java

Enter your regex: dog
Enter input string to search: DoGDOg
I found the text "DoG" starting at index 0 and ending at index 3.
I found the text "DOg" starting at index 3 and ending at index 6.

```

正如你所看到的，字符串字面量"dog"匹配两个实例，不考虑大小写。要编译带有多个标志的模式，请使用按位 OR 运算符"`|`"分隔要包含的标志。为了清晰起见，以下代码示例硬编码正则表达式而不是从`Console`中读取：

```java

pattern = Pattern.compile("[az]$", Pattern.MULTILINE | Pattern.UNIX_LINES);

```

你也可以指定一个`int`变量：

```java

final int flags = Pattern.CASE_INSENSITIVE | Pattern.UNICODE_CASE;
Pattern pattern = Pattern.compile("aa", flags);

```

## 嵌入式标志表达式

也可以使用*嵌入式标志表达式*来启用各种标志。嵌入式标志表达式是`compile`的两参数版本的替代方案，并在正则表达式本身中指定。以下示例使用原始测试工具`RegexTestHarness.java`与嵌入式标志表达式`(?i)`来启用大小写不敏感匹配。

```java

Enter your regex: (?i)foo
Enter input string to search: FOOfooFoOfoO
I found the text "FOO" starting at index 0 and ending at index 3.
I found the text "foo" starting at index 3 and ending at index 6.
I found the text "FoO" starting at index 6 and ending at index 9.
I found the text "foO" starting at index 9 and ending at index 12.

```

再次，不管大小写，所有匹配都成功。

对应于`Pattern`的公共可访问字段的嵌入式标志表达式如下表所示：

| 常量 | 等效的嵌入式标志表达式 |
| --- | --- |
| `Pattern.CANON_EQ` | 无 |
| `Pattern.CASE_INSENSITIVE` | `(?i)` |
| `Pattern.COMMENTS` | `(?x)` |
| `Pattern.MULTILINE` | `(?m)` |
| `Pattern.DOTALL` | `(?s)` |
| `Pattern.LITERAL` | 无 |
| `Pattern.UNICODE_CASE` | `(?u)` |
| `Pattern.UNIX_LINES` | `(?d)` |

## 使用`matches(String,CharSequence)`方法

`Pattern`类定义了一个方便的[`matches`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#matches-java.lang.String-java.lang.CharSequence-)方法，允许您快速检查给定输入字符串中是否存在模式。与所有公共静态方法一样，您应该通过其类名调用`matches`，例如`Pattern.matches("\\d","1");`。在这个例子中，该方法返回`true`，因为数字"1"与正则表达式`\d`匹配。

## 使用`split(String)`方法

[`split`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#split-java.lang.CharSequence-)方法是一个很好的工具，用于收集与已匹配模式两侧的文本。如下所示，在`SplitDemo.java`中，`split`方法可以从字符串"`one:two:three:four:five`"中提取单词"`one two three four five`"：

```java

import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class SplitDemo {

    private static final String REGEX = ":";
    private static final String INPUT =
        "one:two:three:four:five";

    public static void main(String[] args) {
        Pattern p = Pattern.compile(REGEX);
        String[] items = p.split(INPUT);
        for(String s : items) {
            System.out.println(s);
        }
    }
}

```

```java
OUTPUT:

one
two
three
four
five

```

为简单起见，我们匹配了一个字符串字面量，即冒号（`:`），而不是一个复杂的正则表达式。由于我们仍在使用`Pattern`和`Matcher`对象，您可以使用 split 来获取任何正则表达式两侧的文本。这里是相同的示例，`SplitDemo2.java`，修改为按数字拆分：

```java

import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class SplitDemo2 {

    private static final String REGEX = "\\d";
    private static final String INPUT =
        "one9two4three7four1five";

    public static void main(String[] args) {
        Pattern p = Pattern.compile(REGEX);
        String[] items = p.split(INPUT);
        for(String s : items) {
            System.out.println(s);
        }
    }
}

```

```java
OUTPUT:

one
two
three
four
five

```

## 其他实用方法

您可能也会发现以下方法有些用处：

+   [`public static String quote(String s)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#quote-java.lang.String-) 为指定的`String`返回一个字面模式`String`。此方法生成一个`String`，可用于创建一个`Pattern`，该`Pattern`将匹配`String s`，就好像它是一个字面模式。输入序列中的元字符或转义序列将不被赋予特殊含义。

+   [`public String toString()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#toString--) 返回此模式的`String`表示。这是编译此模式的正则表达式。

## `java.lang.String`中的`Pattern`方法等效项

通过几种模仿`java.util.regex.Pattern`行为的方法，`java.lang.String`中也存在正则表达式支持。为方便起见，以下是它们 API 中的关键摘录。

+   [`public boolean matches(String regex)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#matches-java.lang.String-): 告诉这个字符串是否与给定的正则表达式匹配。形式为`*str*.matches(*regex*)`的此方法的调用产生与表达式`Pattern.matches(*regex*, *str*)`完全相同的结果。

+   [`public String[] split(String regex, int limit)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#split-java.lang.String-int-): 将此字符串围绕给定正则表达式的匹配项拆分。形式为`*str*.split(*regex*, *n*)`的此方法的调用产生与表达式`Pattern.compile(*regex*).split(*str*, *n*)`相同的结果。

+   [`public String[] split(String regex)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#split-java.lang.String-): 将此字符串围绕给定正则表达式的匹配项拆分。此方法的工作方式与使用给定表达式和限制参数为零调用两参数拆分方法相同。结果数组中不包括尾随空字符串。

还有一个替换方法，用另一个`CharSequence`替换一个：

+   [`public String replace(CharSequence target,CharSequence replacement)`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#replace-java.lang.CharSequence-java.lang.CharSequence-): 用指定的文字替换序列替换此字符串的每个与字面目标序列匹配的子字符串。替换从字符串的开头到结尾进行，例如，在字符串“aaa”中用“b”替换“aa”将导致“ba”而不是“ab”。
