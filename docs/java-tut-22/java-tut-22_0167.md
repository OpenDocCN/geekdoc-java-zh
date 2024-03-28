# 字符流

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/charstreams.html`](https://docs.oracle.com/javase/tutorial/essential/io/charstreams.html)

Java 平台使用 Unicode 约定存储字符值。字符流 I/O 会自动将内部格式与本地字符集进行转换。在西方区域，本地字符集通常是 ASCII 的 8 位超集。

对于大多数应用程序，使用字符流进行 I/O 与使用字节流进行 I/O 并无太大区别。使用流类进行的输入和输出会自动转换为本地字符集。使用字符流而不是字节流的程序会自动适应本地字符集，并且为国际化做好准备，而无需程序员额外努力。

如果国际化不是首要任务，您可以简单地使用字符流类，而不必过多关注字符集问题。稍后，如果国际化成为首要任务，您的程序可以在不进行大量重编码的情况下进行调整。查看国际化教程以获取更多信息。

## 使用字符流

所有字符流类都是从[`Reader`](https://docs.oracle.com/javase/8/docs/api/java/io/Reader.html)和[`Writer`](https://docs.oracle.com/javase/8/docs/api/java/io/Writer.html)继承而来。与字节流一样，有专门用于文件 I/O 的字符流类：[`FileReader`](https://docs.oracle.com/javase/8/docs/api/java/io/FileReader.html)和[`FileWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/FileWriter.html)。`CopyCharacters`示例演示了这些类。

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class CopyCharacters {
    public static void main(String[] args) throws IOException {

        FileReader inputStream = null;
        FileWriter outputStream = null;

        try {
            inputStream = new FileReader("xanadu.txt");
            outputStream = new FileWriter("characteroutput.txt");

            int c;
            while ((c = inputStream.read()) != -1) {
                outputStream.write(c);
            }
        } finally {
            if (inputStream != null) {
                inputStream.close();
            }
            if (outputStream != null) {
                outputStream.close();
            }
        }
    }
}

```

`CopyCharacters`与`CopyBytes`非常相似。最重要的区别在于，`CopyCharacters`使用`FileReader`和`FileWriter`进行输入和输出，而不是`FileInputStream`和`FileOutputStream`。请注意，`CopyBytes`和`CopyCharacters`都使用一个`int`变量进行读取和写入。但是，在`CopyCharacters`中，`int`变量在其最后 16 位中保存字符值；而在`CopyBytes`中，`int`变量在其最后 8 位中保存`byte`值。

### 使用字节流的字符流

字符流通常是字节流的“包装器”。字符流使用字节流执行物理 I/O，而字符流处理字符和字节之间的转换。例如，`FileReader`使用`FileInputStream`，而`FileWriter`使用`FileOutputStream`。

有两个通用的字节到字符的“桥梁”流：[`InputStreamReader`](https://docs.oracle.com/javase/8/docs/api/java/io/InputStreamReader.html)和[`OutputStreamWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/OutputStreamWriter.html)。当没有符合您需求的预打包字符流类时，请使用它们来创建字符流。网络教程中的套接字课程展示了如何从套接字类提供的字节流创建字符流。

## 面向行的 I/O

字符 I/O 通常以比单个字符更大的单位进行。一个常见的单位是行：一串带有行终止符的字符。行终止符可以是回车/换行序列(`"\r\n"`)，单个回车(`"\r"`)或单个换行(`"\n"`)。支持所有可能的行终止符允许程序读取在任何广泛使用的操作系统上创建的文本文件。

让我们修改`CopyCharacters`示例以使用面向行的 I/O。为此，我们必须使用两个以前未见过的类，[`BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)和[`PrintWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html)。我们将在缓冲 I/O 和格式化中更深入地探讨这些类。现在，我们只关注它们对面向行的 I/O 的支持。

`CopyLines`示例调用`BufferedReader.readLine`和`PrintWriter.println`来逐行进行输入和输出。

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.BufferedReader;
import java.io.PrintWriter;
import java.io.IOException;

public class CopyLines {
    public static void main(String[] args) throws IOException {

        BufferedReader inputStream = null;
        PrintWriter outputStream = null;

        try {
            inputStream = new BufferedReader(new FileReader("xanadu.txt"));
            outputStream = new PrintWriter(new FileWriter("characteroutput.txt"));

            String l;
            while ((l = inputStream.readLine()) != null) {
                outputStream.println(l);
            }
        } finally {
            if (inputStream != null) {
                inputStream.close();
            }
            if (outputStream != null) {
                outputStream.close();
            }
        }
    }
}

```

调用`readLine`返回带有行的文本。`CopyLines`使用`println`输出每一行，该方法会附加当前操作系统的行终止符。这可能与输入文件中使用的行终止符不同。

除了字符和行之外，还有许多结构化文本输入和输出的方式。更多信息，请参见扫描和格式化。
