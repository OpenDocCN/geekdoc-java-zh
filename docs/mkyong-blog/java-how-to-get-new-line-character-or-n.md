# Java–如何获得新的行字符或\n？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-get-new-line-character-or-n/>

[换行符](http://web.archive.org/web/20220619003336/https://en.wikipedia.org/wiki/Newline)，也称为行尾(EOL)、换行符、换行符、行分隔符或回车符，是一个[控制符](http://web.archive.org/web/20220619003336/https://en.wikipedia.org/wiki/Control_character)，用来告知一行文本的结束，下一个字符应该从新行开始。在 Windows 系统上是`\r\n`，在 Linux 系统上是`\n`。

在 Java 中，我们可以使用 [System.lineSeparator()](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#lineSeparator--) 来获取一个与平台相关的换行符。

GetNewLine.java

```java
 package com.mkyong.io.howto;

public class GetNewLine {

    public static void main(String[] args) {

        System.out.print("Line 1");

        // Windows \r\n, Linux \n
        System.out.print(System.lineSeparator());

        System.out.print("Line 2");

    }
} 
```

输出

```java
 Line 1
Line 2 
```

*P . S`System.getProperty("line.separator")`也返回相同的结果。*

## 1.System.lineSeparator()

下面是从 Java 1.7 开始可用的`System.lineSeparator()`方法签名，请阅读注释。

System.java

```java
 package java.lang;

public final class System {

  /**
   * Returns the system-dependent line separator string.  It always
   * returns the same value - the initial value of the {@linkplain
   * #getProperty(String) system property} {@code line.separator}.
   *
   * <p>On UNIX systems, it returns {@code "\n"}; on Microsoft
   * Windows systems it returns {@code "\r\n"}.
   *
   * @return the system-dependent line separator string
   * @since 1.7
   */
  public static String lineSeparator() {
      return lineSeparator;
  } 
```

## 2.打印换行符

如果我们想打印一个平台相关的新行字符，尝试在 [String.format](http://web.archive.org/web/20220619003336/https://mkyong.com/java/java-string-format-examples/) 中的`%n`(行分隔符)。

GetNewLine2.java

```java
 package com.mkyong.io.howto;

public class GetNewLine2 {

    public static void main(String[] args) {

        System.out.print(String.format("%s%n%s", "Line 1", "Line 2"));

    }
} 
```

输出

```java
 Line 1
Line 2 
```

## 3.写一个换行符

如果我们使用`BufferedWriter`向文件中写入文本，那么`newLine()`代表平台相关的换行符。

GetNewLine3.java

```java
 package com.mkyong.io.howto;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

public class GetNewLine3 {

    public static void main(String[] args) {

        // write a new line character
        String fileName = "/home/mkyong/sample.txt";

        List<String> content = Arrays.asList("A", "B", "C");

        try (BufferedWriter bw = new BufferedWriter(new FileWriter(fileName))) {

            for (String t : content) {
                bw.write(t);
                bw.newLine();
                // same
                //bw.write(System.lineSeparator());
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
} 
```

审查`newLine()`方法签名，在内部，`BufferedWriter`也使用`System.lineSeparator()`。

BufferedWriter.java

```java
 package java.io;

public class BufferedWriter extends Writer {
    /**
     * Writes a line separator.  The line separator string is defined by the
     * system property {@code line.separator}, and is not necessarily a single
     * newline ('\n') character.
     *
     * @exception  IOException  If an I/O error occurs
     */
    public void newLine() throws IOException {
        write(System.lineSeparator());
    }

    //...
} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [System.lineSeparator()](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#lineSeparator--)
*   [String.format](http://web.archive.org/web/20220619003336/https://mkyong.com/java/java-string-format-examples/)
*   [格式化程序 JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html)
*   [BufferedWriter JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/BufferedWriter.html)

<input type="hidden" id="mkyong-current-postId" value="16019">