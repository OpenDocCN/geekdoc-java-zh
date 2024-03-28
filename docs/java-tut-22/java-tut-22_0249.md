# 边界匹配器

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/bounds.html`](https://docs.oracle.com/javase/tutorial/essential/regex/bounds.html)

到目前为止，我们只关心匹配是否在特定输入字符串的*某个位置*发现。我们从不关心匹配发生在字符串的*哪个位置*。

通过使用*边界匹配器*，您可以使模式匹配更加精确。例如，也许您对查找特定单词感兴趣，但只有在它出现在行首或行尾时才匹配。或者您想知道匹配是否发生在单词边界上，或者在前一个匹配的末尾。

以下表格列出并解释了所有的边界匹配器。

| 边界构造 | 描述 |
| --- | --- |
| `^` | 一行的开头 |
| `$` | 一行的结尾 |
| `\b` | 单词边界 |
| `\B` | 非单词边界 |
| `\A` | 输入的开头 |
| `\G` | 前一个匹配的结尾 |
| `\Z` | 输入的结尾，但不包括最终终止符（如果有） |
| `\z` | 输入的结尾 |

以下示例演示了边界匹配器`^`和`$`的使用。如上所述，`^`匹配行的开头，`$`匹配行的结尾。

```java

Enter your regex: ^dog$
Enter input string to search: dog
I found the text "dog" starting at index 0 and ending at index 3.

Enter your regex: ^dog$
Enter input string to search:       dog
No match found.

Enter your regex: \s*dog$
Enter input string to search:             dog
I found the text "            dog" starting at index 0 and ending at index 15.

Enter your regex: ^dog\w*
Enter input string to search: dogblahblah
I found the text "dogblahblah" starting at index 0 and ending at index 11.

```

第一个示例成功，因为模式占据了整个输入字符串。第二个示例失败，因为输入字符串开头包含额外的空格。第三个示例指定了一个允许无限空格，然后在行尾跟着"dog"的表达式。第四个示例要求"dog"必须出现在一行的开头，后面跟着无限数量的单词字符。

要检查模式是否从单词边界开始和结束（而不是在更长字符串中的子字符串），只需在两侧使用`\b`；例如，`\bdog\b`

```java

Enter your regex: \bdog\b
Enter input string to search: The dog plays in the yard.
I found the text "dog" starting at index 4 and ending at index 7.

Enter your regex: \bdog\b
Enter input string to search: The doggie plays in the yard.
No match found.

```

要在非单词边界上匹配表达式，请使用`\B`：

```java

Enter your regex: \bdog\B
Enter input string to search: The dog plays in the yard.
No match found.

Enter your regex: \bdog\B
Enter input string to search: The doggie plays in the yard.
I found the text "dog" starting at index 4 and ending at index 7.

```

要求匹配仅发生在前一个匹配的末尾，请使用`\G`：

```java

Enter your regex: dog 
Enter input string to search: dog dog
I found the text "dog" starting at index 0 and ending at index 3.
I found the text "dog" starting at index 4 and ending at index 7.

Enter your regex: \Gdog 
Enter input string to search: dog dog
I found the text "dog" starting at index 0 and ending at index 3.

```

这里第二个示例只找到一个匹配，因为第二次出现的"dog"不是从前一个匹配的末尾开始的。
