# 字符类

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/char_classes.html`](https://docs.oracle.com/javase/tutorial/essential/regex/char_classes.html)

如果您浏览[`Pattern`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)类的规范，您会看到总结支持的正则表达式构造的表格。在“字符类”部分，您会找到以下内容：

| 构造 | 描述 |
| --- | --- |
| `[abc]` | a、b 或 c（简单类） |
| `[^abc]` | 除了 a、b 或 c 之外的任何字符（否定） |
| `[a-zA-Z]` | a 到 z，或 A 到 Z，包括（范围） |
| `[a-d[m-p]]` | a 到 d，或者 m 到 p：[a-dm-p]（并集） |
| `[a-z&&[def]]` | d、e 或 f（交集） |
| `[a-z&&[^bc]]` | a 到 z，除了 b 和 c：[ad-z]（减法） |
| `[a-z&&[^m-p]]` | a 到 z，但不包括 m 到 p：[a-lq-z]（减法） |

左侧列指定了正则表达式构造，而右侧列描述了每个构造将匹配的条件。

* * *

**注意：**短语“字符类”中的“类”并不指代`.class`文件。在正则表达式的上下文中，*字符类*是一组方括号内的字符。它指定了成功匹配给定输入字符串中的单个字符的字符。

* * *

## 简单类

字符类的最基本形式是在方括号内简单地放置一组字符。例如，正则表达式`[bcr]at`将匹配单词"bat"、"cat"或"rat"，因为它定义了一个字符类（接受"b"、"c"或"r"中的任意一个）作为其第一个字符。

```java

Enter your regex: [bcr]at
Enter input string to search: bat
I found the text "bat" starting at index 0 and ending at index 3.

Enter your regex: [bcr]at
Enter input string to search: cat
I found the text "cat" starting at index 0 and ending at index 3.

Enter your regex: [bcr]at
Enter input string to search: rat
I found the text "rat" starting at index 0 and ending at index 3.

Enter your regex: [bcr]at
Enter input string to search: hat
No match found.

```

在上面的例子中，只有当第一个字母与字符类定义的字符之一匹配时，整体匹配才成功。

### 否定

要匹配除列出的字符之外的所有字符，请在字符类的开头插入“`^`”元字符。这种技术被称为*否定*。

```java

Enter your regex: [^bcr]at
Enter input string to search: bat
No match found.

Enter your regex: [^bcr]at
Enter input string to search: cat
No match found.

Enter your regex: [^bcr]at
Enter input string to search: rat
No match found.

Enter your regex: [^bcr]at
Enter input string to search: hat
I found the text "hat" starting at index 0 and ending at index 3.

```

只有当输入字符串的第一个字符*不*包含字符类定义的任何字符时，匹配才成功。

### 范围

有时，您可能想要定义一个包含一系列值的字符类，比如字母"a 到 h"或数字"1 到 5"。要指定一个范围，只需在要匹配的第一个和最后一个字符之间插入“`-`”元字符，比如`[1-5]`或`[a-h]`。您还可以在类内将不同的范围放在一起，以进一步扩展匹配可能性。例如，`[a-zA-Z]`将匹配任何字母：a 到 z（小写）或 A 到 Z（大写）。

以下是一些范围和否定的示例： 

```java
Enter your regex: [a-c]
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: [a-c]
Enter input string to search: b
I found the text "b" starting at index 0 and ending at index 1.

Enter your regex: [a-c]
Enter input string to search: c
I found the text "c" starting at index 0 and ending at index 1.

Enter your regex: [a-c]
Enter input string to search: d
No match found.

Enter your regex: foo[1-5]
Enter input string to search: foo1
I found the text "foo1" starting at index 0 and ending at index 4.

Enter your regex: foo[1-5]
Enter input string to search: foo5
I found the text "foo5" starting at index 0 and ending at index 4.

Enter your regex: foo[1-5]
Enter input string to search: foo6
No match found.

Enter your regex: foo[¹-5]
Enter input string to search: foo1
No match found.

Enter your regex: foo[¹-5]
Enter input string to search: foo6
I found the text "foo6" starting at index 0 and ending at index 4.

```

### 并集

您还可以使用*并集*来创建由两个或多个单独字符类组成的单一字符类。要创建并集，只需将一个类嵌套在另一个类中，比如`[0-4[6-8]]`。这个特定的并集创建了一个单一的字符类，匹配数字 0、1、2、3、4、6、7 和 8。

```java
Enter your regex: [0-4[6-8]]
Enter input string to search: 0
I found the text "0" starting at index 0 and ending at index 1.

Enter your regex: [0-4[6-8]]
Enter input string to search: 5
No match found.

Enter your regex: [0-4[6-8]]
Enter input string to search: 6
I found the text "6" starting at index 0 and ending at index 1.

Enter your regex: [0-4[6-8]]
Enter input string to search: 8
I found the text "8" starting at index 0 and ending at index 1.

Enter your regex: [0-4[6-8]]
Enter input string to search: 9
No match found.

```

### 交集

要创建一个仅匹配所有嵌套类共同字符的单一字符类，请使用`&&`，比如`[0-9&&[345]]`。这个特定的交集创建了一个单一的字符类，仅匹配两个字符类共同的数字：3、4 和 5。

```java

Enter your regex: [0-9&&[345]]
Enter input string to search: 3
I found the text "3" starting at index 0 and ending at index 1.

Enter your regex: [0-9&&[345]]
Enter input string to search: 4
I found the text "4" starting at index 0 and ending at index 1.

Enter your regex: [0-9&&[345]]
Enter input string to search: 5
I found the text "5" starting at index 0 and ending at index 1.

Enter your regex: [0-9&&[345]]
Enter input string to search: 2
No match found.

Enter your regex: [0-9&&[345]]
Enter input string to search: 6
No match found.

```

这里有一个示例，展示了两个范围的交集：

```java

Enter your regex: [2-8&&[4-6]]
Enter input string to search: 3
No match found.

Enter your regex: [2-8&&[4-6]]
Enter input string to search: 4
I found the text "4" starting at index 0 and ending at index 1.

Enter your regex: [2-8&&[4-6]]
Enter input string to search: 5
I found the text "5" starting at index 0 and ending at index 1.

Enter your regex: [2-8&&[4-6]]
Enter input string to search: 6
I found the text "6" starting at index 0 and ending at index 1.

Enter your regex: [2-8&&[4-6]]
Enter input string to search: 7
No match found.

```

### 减法

最后，您可以使用*减法*来否定一个或多个嵌套的字符类，比如`[0-9&&[³⁴⁵]]`。这个示例创建了一个单一的字符类，匹配从 0 到 9 的所有内容，*除了*数字 3、4 和 5。

```java

Enter your regex: [0-9&&[³⁴⁵]]
Enter input string to search: 2
I found the text "2" starting at index 0 and ending at index 1.

Enter your regex: [0-9&&[³⁴⁵]]
Enter input string to search: 3
No match found.

Enter your regex: [0-9&&[³⁴⁵]]
Enter input string to search: 4
No match found.

Enter your regex: [0-9&&[³⁴⁵]]
Enter input string to search: 5
No match found.

Enter your regex: [0-9&&[³⁴⁵]]
Enter input string to search: 6
I found the text "6" starting at index 0 and ending at index 1.

Enter your regex: [0-9&&[³⁴⁵]]
Enter input string to search: 9
I found the text "9" starting at index 0 and ending at index 1.

```

现在我们已经介绍了如何创建字符类，您可能想在继续下一节之前查看字符类表。
