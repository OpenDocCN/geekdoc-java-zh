# 扫描

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/scanning.html`](https://docs.oracle.com/javase/tutorial/essential/io/scanning.html)

类型为[`Scanner`](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)的对象对于将格式化输入拆分为标记并根据其数据类型翻译单个标记非常有用。

## 将输入分解为标记

默认情况下，扫描器使用空白字符来分隔标记。（空白字符包括空格、制表符和行终止符。有关完整列表，请参考[`Character.isWhitespace`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isWhitespace-char-)的文档。）为了了解扫描的工作原理，让我们看看`ScanXan`，一个程序，它读取`xanadu.txt`中的单词并将它们逐行打印出来。

```java
import java.io.*;
import java.util.Scanner;

public class ScanXan {
    public static void main(String[] args) throws IOException {

        Scanner s = null;

        try {
            s = new Scanner(new BufferedReader(new FileReader("xanadu.txt")));

            while (s.hasNext()) {
                System.out.println(s.next());
            }
        } finally {
            if (s != null) {
                s.close();
            }
        }
    }
}

```

请注意，当`ScanXan`完成对扫描器对象的操作时，会调用`Scanner`的`close`方法。即使扫描器不是一个流，你也需要关闭它以表示你已经完成了对其底层流的操作。

`ScanXan`的输出如下所示：

```java
In
Xanadu
did
Kubla
Khan
A
stately
pleasure-dome
...

```

要使用不同的标记分隔符，调用`useDelimiter()`，指定一个正则表达式。例如，假设您希望标记分隔符是逗号，后面可以跟随空白。您可以调用，

```java
s.useDelimiter(",\\s*");

```

## 翻译单个标记

`ScanXan`示例将所有输入标记视为简单的`String`值。`Scanner`还支持所有 Java 语言的基本类型（除了`char`），以及`BigInteger`和`BigDecimal`。此外，数值可以使用千位分隔符。因此，在`US`区域设置中，`Scanner`可以正确地将字符串"32,767"读取为整数值。

我们必须提及区域设置，因为千位分隔符和小数符是与区域设置相关的。因此，如果我们没有指定扫描器应该使用`US`区域设置，下面的示例在所有区域设置中都不会正确工作。这通常不是您需要担心的事情，因为您的输入数据通常来自与您相同区域设置的源。但是，这个示例是 Java 教程的一部分，会分发到世界各地。

`ScanSum`示例读取一组`double`值并将它们相加。以下是源代码：

```java
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.IOException;
import java.util.Scanner;
import java.util.Locale;

public class ScanSum {
    public static void main(String[] args) throws IOException {

        Scanner s = null;
        double sum = 0;

        try {
            s = new Scanner(new BufferedReader(new FileReader("usnumbers.txt")));
            s.useLocale(Locale.US);

            while (s.hasNext()) {
                if (s.hasNextDouble()) {
                    sum += s.nextDouble();
                } else {
                    s.next();
                }   
            }
        } finally {
            s.close();
        }

        System.out.println(sum);
    }
}

```

这是示例输入文件，`usnumbers.txt`

```java
8.5
32,767
3.14159
1,000,000.1

```

输出字符串为"1032778.74159"。在某些区域设置中，句号可能是不同的字符，因为`System.out`是一个`PrintStream`对象，该类不提供覆盖默认区域设置的方法。我们可以为整个程序覆盖区域设置，或者我们可以使用格式化，如下一主题中所述，格式化。
