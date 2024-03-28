# 捕获组

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/groups.html`](https://docs.oracle.com/javase/tutorial/essential/regex/groups.html)

在上一节中，我们看到量词如何附加到一个字符、字符类或捕获组上。但直到现在，我们还没有详细讨论捕获组的概念。

*捕获组*是将多个字符视为单个单元的一种方式。它们是通过将要分组的字符放在一对括号中创建的。例如，正则表达式`(dog)`创建了一个包含字母`"d" "o"`和`"g"`的单个组。与捕获组匹配的输入字符串部分将被保存在内存中，以便稍后通过反向引用进行调用（如下面在反向引用部分中讨论）。

## 编号

如[`Pattern`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) API 中所述，通过从左到右计算其开括号来对捕获组进行编号。例如，在表达式`((A)(B(C)))`中，有四个这样的组：

1.  `((A)(B(C)))`

1.  `(A)`

1.  `(B(C))`

1.  `(C)`

要找出表达式中存在多少组，请在匹配器对象上调用`groupCount`方法。`groupCount`方法返回一个`int`，显示匹配器模式中存在的捕获组数。在这个例子中，`groupCount`将返回数字`4`，显示该模式包含 4 个捕获组。

还有一个特殊组，组 0，它始终表示整个表达式。这个组不包括在`groupCount`报告的总数中。以`(?`开头的组是纯粹的、*非捕获组*，不捕获文本，也不计入组总数。（您将在 Pattern 类的方法部分后面看到非捕获组的示例。）

重要的是要理解组是如何编号的，因为一些`Matcher`方法接受一个指定特定组号的`int`作为参数：

+   [`public int start(int group)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#start-int-)：返回在先前匹配操作期间由给定组捕获的子序列的起始索引。

+   [`public int end (int group)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#end-int-)：返回在先前匹配操作期间由给定组捕获的子序列的最后一个字符的索引加一。

+   [`public String group (int group)`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#group-int-)：返回在先前匹配操作期间由给定组捕获的输入子序列。

## 反向引用

匹配捕获组的输入字符串部分将被保存在内存中，以便通过*反向引用*进行后续调用。反向引用在正则表达式中被指定为反斜杠（`\`）后跟表示要调用的组的数字。例如，表达式`(\d\d)`定义了一个捕获组，匹配连续两个数字，可以在表达式中通过反向引用`\1`稍后调用。

要匹配任意两个数字，后跟完全相同的两个数字，您可以使用`(\d\d)\1`作为正则表达式：

```java

Enter your regex: (\d\d)\1
Enter input string to search: 1212
I found the text "1212" starting at index 0 and ending at index 4.

```

如果更改最后两个数字，匹配将失败：

```java

Enter your regex: (\d\d)\1
Enter input string to search: 1234
No match found.

```

对于嵌套的捕获组，反向引用的工作方式完全相同：指定一个反斜杠，后跟要调用的组的编号。
