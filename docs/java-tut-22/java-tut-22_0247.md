# 量词

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/quant.html`](https://docs.oracle.com/javase/tutorial/essential/regex/quant.html)

*量词*允许您指定要匹配的出现次数。为方便起见，下面呈现了 Pattern API 规范的三个部分，描述了贪婪、勉强和占有量词。乍一看，似乎量词`X?`、`X??`和`X?+`做的事情完全相同，因为它们都承诺匹配"`X`，一次或零次"。有微妙的实现差异，将在本节末尾解释。

| 贪婪 | 勉强 | 占有 | 含义 |
| --- | --- | --- | --- |
| `X?` | `X??` | `X?+` | `X`，一次或零次 |
| `X*` | `X*?` | `X*+` | `X`，零次或多次 |
| `X+` | `X+?` | `X++` | `X`，一次或多次 |
| `X{n}` | `X{n}?` | `X{n}+` | `X`，恰好*`n`*次 |
| `X{n,}` | `X{n,}?` | `X{n,}+` | `X`，至少*`n`*次 |
| `X{n,m}` | `X{n,m}?` | `X{n,m}+` | `X`，至少*`n`*次但不超过*`m`*次 |

让我们通过创建三个不同的正则表达式来开始我们对贪婪量词的研究：字母"a"后面跟着`?`、`*`或`+`。让我们看看当这些表达式针对空输入字符串`""`进行测试时会发生什么：

```java

Enter your regex: a?
Enter input string to search: 
I found the text "" starting at index 0 and ending at index 0.

Enter your regex: a*
Enter input string to search: 
I found the text "" starting at index 0 and ending at index 0.

Enter your regex: a+
Enter input string to search: 
No match found.

```

## 零长度匹配

在上面的例子中，前两种情况匹配成功，因为表达式`a?`和`a*`都允许字母`a`出现零次。您还会注意到开始和结束索引都是零，这与我们迄今为止看到的任何示例都不同。空输入字符串`""`没有长度，因此测试只是在索引 0 处匹配到了空。这种匹配称为*零长度匹配*。零长度匹配可以出现在几种情况下：在空输入字符串中，在输入字符串的开头，在输入字符串的最后一个字符之后，或在输入字符串的任意两个字符之间。零长度匹配很容易识别，因为它们始终从相同的索引位置开始和结束。

让我们通过几个例子来探索零长度匹配。将输入字符串更改为单个字母"a"，您会注意到一些有趣的事情：

```java

Enter your regex: a?
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.
I found the text "" starting at index 1 and ending at index 1.

Enter your regex: a*
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.
I found the text "" starting at index 1 and ending at index 1.

Enter your regex: a+
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

```

所有三个量词都找到了字母"a"，但前两个还在索引 1 处找到了零长度匹配；也就是说，在输入字符串的最后一个字符之后。请记住，匹配器将字符"a"视为位于索引 0 和索引 1 之间的单元格中，我们的测试工具循环直到无法找到匹配。根据使用的量词，最后一个字符后面的“无内容”可能会或可能不会触发匹配。

现在将输入字符串更改为连续五次的字母"a"，您将得到以下结果：

```java

Enter your regex: a?
Enter input string to search: aaaaa
I found the text "a" starting at index 0 and ending at index 1.
I found the text "a" starting at index 1 and ending at index 2.
I found the text "a" starting at index 2 and ending at index 3.
I found the text "a" starting at index 3 and ending at index 4.
I found the text "a" starting at index 4 and ending at index 5.
I found the text "" starting at index 5 and ending at index 5.

Enter your regex: a*
Enter input string to search: aaaaa
I found the text "aaaaa" starting at index 0 and ending at index 5.
I found the text "" starting at index 5 and ending at index 5.

Enter your regex: a+
Enter input string to search: aaaaa
I found the text "aaaaa" starting at index 0 and ending at index 5.

```

表达式 `a?` 为每个字符找到一个匹配，因为它匹配 "a" 出现零次或一次时。表达式 `a*` 找到两个单独的匹配：第一个匹配中的所有字母 "a"，然后是索引 5 处最后一个字符后的零长度匹配。最后，`a+` 匹配所有字母 "a" 的出现，忽略最后索引处的 "nothing" 的存在。

此时，您可能想知道如果前两个量词遇到除 "a" 外的字母会发生什么。例如，如果它遇到字母 "b"，比如 "ababaaaab" 会发生什么？

让我们来看看：

```java

Enter your regex: a?
Enter input string to search: ababaaaab
I found the text "a" starting at index 0 and ending at index 1.
I found the text "" starting at index 1 and ending at index 1.
I found the text "a" starting at index 2 and ending at index 3.
I found the text "" starting at index 3 and ending at index 3.
I found the text "a" starting at index 4 and ending at index 5.
I found the text "a" starting at index 5 and ending at index 6.
I found the text "a" starting at index 6 and ending at index 7.
I found the text "a" starting at index 7 and ending at index 8.
I found the text "" starting at index 8 and ending at index 8.
I found the text "" starting at index 9 and ending at index 9.

Enter your regex: a*
Enter input string to search: ababaaaab
I found the text "a" starting at index 0 and ending at index 1.
I found the text "" starting at index 1 and ending at index 1.
I found the text "a" starting at index 2 and ending at index 3.
I found the text "" starting at index 3 and ending at index 3.
I found the text "aaaa" starting at index 4 and ending at index 8.
I found the text "" starting at index 8 and ending at index 8.
I found the text "" starting at index 9 and ending at index 9.

Enter your regex: a+
Enter input string to search: ababaaaab
I found the text "a" starting at index 0 and ending at index 1.
I found the text "a" starting at index 2 and ending at index 3.
I found the text "aaaa" starting at index 4 and ending at index 8.

```

尽管字母 "b" 出现在单元格 1、3 和 8 中，输出报告在这些位置上的零长度匹配。正则表达式 `a?` 并不是专门寻找字母 "b"；它只是查找字母 "a" 的存在（或不存在）。如果量词允许 "a" 匹配零次，那么输入字符串中不是 "a" 的任何内容都会显示为零长度匹配。剩下的 a 根据前面示例中讨论的规则进行匹配。

要精确匹配一个模式 *n* 次，只需在大括号内指定数字：

```java

Enter your regex: a{3}
Enter input string to search: aa
No match found.

Enter your regex: a{3}
Enter input string to search: aaa
I found the text "aaa" starting at index 0 and ending at index 3.

Enter your regex: a{3}
Enter input string to search: aaaa
I found the text "aaa" starting at index 0 and ending at index 3.

```

在这里，正则表达式 `a{3}` 在连续三个字母 "a" 中搜索三次出现的情况。第一个测试失败，因为输入字符串中没有足够的 a 与之匹配。第二个测试包含输入字符串中恰好 3 个 a，触发了匹配。第三个测试也触发了匹配，因为输入字符串开头恰好有 3 个 a。在此之后的任何内容都与第一个匹配无关。如果在那之后再次出现该模式，将触发后续的匹配：

```java

Enter your regex: a{3}
Enter input string to search: aaaaaaaaa
I found the text "aaa" starting at index 0 and ending at index 3.
I found the text "aaa" starting at index 3 and ending at index 6.
I found the text "aaa" starting at index 6 and ending at index 9.

```

要求模式至少出现 *n* 次，数字后加逗号：

```java

Enter your regex: a{3,}
Enter input string to search: aaaaaaaaa
I found the text "aaaaaaaaa" starting at index 0 and ending at index 9.

```

使用相同的输入字符串，此测试仅找到一个匹配，因为连续 9 个 a 满足 "至少" 3 个 a 的需求。

最后，要指定出现次数的上限，在大括号内添加第二个数字：

```java

Enter your regex: a{3,6} // find at least 3 (but no more than 6) a's in a row
Enter input string to search: aaaaaaaaa
I found the text "aaaaaa" starting at index 0 and ending at index 6.
I found the text "aaa" starting at index 6 and ending at index 9.

```

第一个匹配强制停在 6 个字符的上限处。第二个匹配包括剩下的内容，这恰好是三个 a，这是允许的最小字符数。如果输入字符串短了一个字符，就不会有第二个匹配，因为只剩下两个 a。

## 捕获组和带量词的字符类

到目前为止，我们只在包含一个字符的输入字符串上测试了量词。实际上，量词只能附加到一个字符上，因此正则表达式 "abc+" 意味着 "a，后跟 b，后跟 c 一次或多次"。它不意味着 "abc" 一次或多次。然而，量词也可以附加到字符类和捕获组，例如 `[abc]+`（a 或 b 或 c，一次或多次）或 `(abc)+`（组 "abc"，一次或多次）。

让我们通过连续三次指定组`(dog)`来说明。

```java

Enter your regex: (dog){3}
Enter input string to search: dogdogdogdogdogdog
I found the text "dogdogdog" starting at index 0 and ending at index 9.
I found the text "dogdogdog" starting at index 9 and ending at index 18.

Enter your regex: dog{3}
Enter input string to search: dogdogdogdogdogdog
No match found.

```

在第一个例子中找到三个匹配，因为量词应用于整个捕获组。然而，去掉括号，匹配失败，因为量词`{3}`现在仅适用于字母"g"。

同样，我们可以将一个量词应用于整个字符类：

```java
Enter your regex: [abc]{3}
Enter input string to search: abccabaaaccbbbc
I found the text "abc" starting at index 0 and ending at index 3.
I found the text "cab" starting at index 3 and ending at index 6.
I found the text "aaa" starting at index 6 and ending at index 9.
I found the text "ccb" starting at index 9 and ending at index 12.
I found the text "bbc" starting at index 12 and ending at index 15.

Enter your regex: abc{3}
Enter input string to search: abccabaaaccbbbc
No match found.

```

在第一个例子中，量词`{3}`应用于整个字符类，但在第二个例子中仅应用于字母"c"。

## 贪婪、勉强和占有量词之间的区别

贪婪、勉强和占有量词之间存在微妙的区别。

贪婪量词被认为是"贪婪"的，因为它们强制匹配器在尝试第一次匹配之前读取或*吞掉*整个输入字符串。如果第一次匹配尝试（整个输入字符串）失败，匹配器会向后退一个字符并重试，重复这个过程直到找到匹配或没有更多字符可以后退。根据表达式中使用的量词，它将尝试匹配的最后一件事是 1 个或 0 个字符。

然而，勉强量词采取相反的方法：它们从输入字符串的开头开始，然后不情愿地一次吃掉一个字符寻找匹配。它们尝试的最后一件事是整个输入字符串。

最后，占有量词总是吞掉整个输入字符串，尝试一次（仅一次）匹配。与贪婪量词不同，占有量词永远不会后退，即使这样做可以使整体匹配成功。

为了说明，考虑输入字符串`xfooxxxxxxfoo`。

```java

Enter your regex: .*foo  // greedy quantifier
Enter input string to search: xfooxxxxxxfoo
I found the text "xfooxxxxxxfoo" starting at index 0 and ending at index 13.

Enter your regex: .*?foo  // reluctant quantifier
Enter input string to search: xfooxxxxxxfoo
I found the text "xfoo" starting at index 0 and ending at index 4.
I found the text "xxxxxxfoo" starting at index 4 and ending at index 13.

Enter your regex: .*+foo // possessive quantifier
Enter input string to search: xfooxxxxxxfoo
No match found.

```

第一个例子使用贪婪量词`.*`来找到"任何东西"，零次或多次，然后是字母`"f" "o" "o"`。因为量词是贪婪的，表达式的`.*`部分首先吞掉整个输入字符串。此时，整体表达式无法成功，因为最后三个字母（`"f" "o" "o"`）已经被消耗。因此，匹配器慢慢地一次后退一个字母，直到最右边的"foo"出现，匹配成功并结束搜索。

然而，第二个例子是勉强的，因此它首先消耗"nothing"。因为"foo"不出现在字符串的开头，它被迫吞下第一个字母（一个"x"），这触发了第一个匹配在 0 和 4。我们的测试工具继续这个过程直到输入字符串耗尽。它在 4 和 13 找到另一个匹配。

第三个例子无法找到匹配，因为量词是占有的。在这种情况下，整个输入字符串被`.*+`消耗，没有剩余内容来满足表达式末尾的"foo"。在想要占有所有内容而永远不后退的情况下使用占有量词；在匹配不立即找到的情况下，它将优于等效的贪婪量词。
