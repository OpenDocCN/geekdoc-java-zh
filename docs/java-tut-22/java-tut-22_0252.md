# PatternSyntaxException 类的方法

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/pse.html`](https://docs.oracle.com/javase/tutorial/essential/regex/pse.html)

[`PatternSyntaxException`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/PatternSyntaxException.html)是一个未经检查的异常，表示正则表达式模式中的语法错误。`PatternSyntaxException`类提供以下方法，帮助您确定出了什么问题：

+   [`public String getDescription()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/PatternSyntaxException.html#getDescription--): 检索错误的描述。

+   [`public int getIndex()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/PatternSyntaxException.html#getIndex--): 检索错误索引。

+   [`public String getPattern()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/PatternSyntaxException.html#getPattern--): 检索错误的正则表达式模式。

+   [`public String getMessage()`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/PatternSyntaxException.html#getMessage--): 返回一个包含语法错误描述、错误索引、错误的正则表达式模式以及模式中错误索引的可视指示的多行字符串。

以下源代码，`RegexTestHarness2.java`，更新了我们的测试工具以检查格式不正确的正则表达式：

```java

import java.io.Console;
import java.util.regex.Pattern;
import java.util.regex.Matcher;
import java.util.regex.PatternSyntaxException;

public class RegexTestHarness2 {

    public static void main(String[] args){
        Pattern pattern = null;
        Matcher matcher = null;

        Console console = System.console();
        if (console == null) {
            System.err.println("No console.");
            System.exit(1);
        }
        while (true) {
            try{
                pattern = 
                Pattern.compile(console.readLine("%nEnter your regex: "));

                matcher = 
                pattern.matcher(console.readLine("Enter input string to search: "));
            }
            catch(PatternSyntaxException pse){
                console.format("There is a problem" +
                               " with the regular expression!%n");
                console.format("The pattern in question is: %s%n",
                               pse.getPattern());
                console.format("The description is: %s%n",
                               pse.getDescription());
                console.format("The message is: %s%n",
                               pse.getMessage());
                console.format("The index is: %s%n",
                               pse.getIndex());
                System.exit(0);
            }
            boolean found = false;
            while (matcher.find()) {
                console.format("I found the text" +
                    " \"%s\" starting at " +
                    "index %d and ending at index %d.%n",
                    matcher.group(),
                    matcher.start(),
                    matcher.end());
                found = true;
            }
            if(!found){
                console.format("No match found.%n");
            }
        }
    }
}

```

要运行此测试，请将`?i)foo`输入为正则表达式。这个错误是程序员在嵌入式标志表达式`(?i)`中忘记开括号的常见情况。这样做将产生以下结果：

```java
Enter your regex: ?i)
There is a problem with the regular expression!
The pattern in question is: ?i)
The description is: Dangling meta character '?'
The message is: Dangling meta character '?' near index 0
?i)
^
The index is: 0

```

从这个输出中，我们可以看到语法错误是在索引 0 处的悬空元字符（问号）。缺少的开括号是罪魁祸首。
