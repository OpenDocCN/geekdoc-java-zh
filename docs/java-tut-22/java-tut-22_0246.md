# 预定义字符类

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/pre_char_classes.html`](https://docs.oracle.com/javase/tutorial/essential/regex/pre_char_classes.html)

[`Pattern`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) API 包含许多有用的*预定义字符类*，提供了常用正则表达式的便捷简写：

| 构造 | 描述 |
| --- | --- |
| `.` | 任何字符（可能匹配行终止符，也可能不匹配） |
| `\d` | 一个数字：`[0-9]` |
| `\D` | 一个非数字：`[⁰-9]` |
| `\s` | 一个空白字符：`[ \t\n\x0B\f\r]` |
| `\S` | 一个非空白字符：`[^\s]` |
| `\w` | 一个单词字符：`[a-zA-Z_0-9]` |
| `\W` | 一个非单词字符：`[^\w]` |

在上表中，左列中的每个构造都是右列中字符类的简写。例如，`\d` 表示数字范围（0-9），`\w` 表示单词字符（任何小写字母、任何大写字母、下划线字符或任何数字）。尽可能使用预定义类。它们使您的代码更易阅读，并消除由格式不正确的字符类引入的错误。

以反斜杠开头的构造称为*转义构造*。我们在字符串文字部分预览了转义构造，其中我们提到了反斜杠和引号的使用。如果您在字符串文字中使用转义构造，必须在字符串编译之前在反斜杠前加上另一个反斜杠。例如：

```java

private final String REGEX = "\\d"; // a single digit

```

在这个例子中，`\d` 是正则表达式；额外的反斜杠是为了代码编译而需要的。测试工具直接从`Console`读取表达式，因此额外的反斜杠是不必要的。

以下示例演示了预定义字符类的使用。

```java

Enter your regex: .
Enter input string to search: @
I found the text "@" starting at index 0 and ending at index 1.

Enter your regex: . 
Enter input string to search: 1
I found the text "1" starting at index 0 and ending at index 1.

Enter your regex: .
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: \d
Enter input string to search: 1
I found the text "1" starting at index 0 and ending at index 1.

Enter your regex: \d
Enter input string to search: a
No match found.

Enter your regex: \D
Enter input string to search: 1
No match found.

Enter your regex: \D
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: \s
Enter input string to search:  
I found the text " " starting at index 0 and ending at index 1.

Enter your regex: \s
Enter input string to search: a
No match found.

Enter your regex: \S
Enter input string to search:  
No match found.

Enter your regex: \S
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: \w
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: \w
Enter input string to search: !
No match found.

Enter your regex: \W
Enter input string to search: a
No match found.

Enter your regex: \W
Enter input string to search: !
I found the text "!" starting at index 0 and ending at index 1.

```

在前三个例子中，正则表达式只是`.`（"点"元字符），表示"任何字符"。因此，在所有三种情况下匹配都成功（随机选择的`@`字符、一个数字和一个字母）。其余的例子每个都使用了预定义字符类表中的一个正则表达式构造。您可以参考这个表格来理解每个匹配背后的逻辑：

+   `\d` 匹配所有数字

+   `\s` 匹配空格

+   `\w` 匹配单词字符

或者，大写字母表示相反的意思：

+   `\D` 匹配非数字

+   `\S` 匹配非空格

+   `\W` 匹配非单词字符
