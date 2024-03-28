# 测试工具

> 原文：[`docs.oracle.com/javase/tutorial/essential/regex/test_harness.html`](https://docs.oracle.com/javase/tutorial/essential/regex/test_harness.html)

这一部分定义了一个可重复使用的测试工具，`RegexTestHarness.java`，用于探索此 API 支持的正则表达式构造。运行此代码的命令是`java RegexTestHarness`；不接受命令行参数。该应用程序将重复循环，提示用户输入正则表达式和输入字符串。使用这个测试工具是可选的，但您可能会发现在探索下面页面讨论的测试用例时很方便。

```java

import java.io.Console;
import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class RegexTestHarness {

    public static void main(String[] args){
        Console console = System.console();
        if (console == null) {
            System.err.println("No console.");
            System.exit(1);
        }
        while (true) {

            Pattern pattern = 
            Pattern.compile(console.readLine("%nEnter your regex: "));

            Matcher matcher = 
            pattern.matcher(console.readLine("Enter input string to search: "));

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

在继续下一部分之前，请保存并编译此代码，以确保您的开发环境支持所需的包。
