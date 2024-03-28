# 文件操作

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/fileOps.html`](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html)

[`Files`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)类是`java.nio.file`包的另一个主要入口点。该类提供了丰富的静态方法集，用于读取、写入和操作文件和目录。`Files`方法适用于`Path`对象的实例。在继续阅读其余部分之前，您应该熟悉以下常见概念：

+   释放系统资源

+   捕获异常

+   可变参数

+   原子操作

+   方法链

+   什么是 Glob？

+   链接感知

## 释放系统资源

此 API 中使用的许多资源，如流或通道，实现或扩展了[`java.io.Closeable`](https://docs.oracle.com/javase/8/docs/api/java/io/Closeable.html)接口。`Closeable`资源的要求是在不再需要时必须调用`close`方法来释放资源。忽略关闭资源可能会对应用程序的性能产生负面影响。下一节描述的`try-`with-resources 语句会为您处理此步骤。

## 捕获异常

在文件 I/O 中，意外情况是生活中的一个事实：文件存在（或不存在）时预期的，程序无法访问文件系统，默认文件系统实现不支持特定功能，等等。可能会遇到许多错误。

所有访问文件系统的方法都可能抛出`IOException`。最佳做法是通过将这些方法嵌入到 Java SE 7 版本中引入的`try-`with-resources 语句中来捕获这些异常。`try-`with-resources 语句的优点是编译器在不再需要时会自动生成关闭资源的代码。以下代码显示了这种情况可能如何：

```java
Charset charset = Charset.forName("US-ASCII");
String s = ...;
try (BufferedWriter writer = Files.newBufferedWriter(file, charset)) {
    writer.write(s, 0, s.length());
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}

```

有关更多信息，请参阅 try-with-resources 语句。

或者，您可以将文件 I/O 方法嵌入到`try`块中，然后在`catch`块中捕获任何异常。如果您的代码已打开任何流或通道，则应在`finally`块中关闭它们。使用 try-catch-finally 方法的前一个示例可能如下所示：

```java
Charset charset = Charset.forName("US-ASCII");
String s = ...;
BufferedWriter writer = null;
try {
    writer = Files.newBufferedWriter(file, charset);
    writer.write(s, 0, s.length());
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
} finally {
    if (writer != null) writer.close();
}

```

有关更多信息，请参阅捕获和处理异常。

除了`IOException`，许多特定异常扩展了[`FileSystemException`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html)。这个类有一些有用的方法，返回涉及的文件[(`getFile`)](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html#getFile--)，详细的消息字符串[(`getMessage`)](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html#getMessage--)，文件系统操作失败的原因[(`getReason`)](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html#getReason--)，以及涉及的“其他”文件（如果有）[(`getOtherFile`)](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html#getOtherFile--)。

以下代码片段展示了`getFile`方法可能如何使用：

```java
try (...) {
    ...    
} catch (NoSuchFileException x) {
    System.err.format("%s does not exist\n", x.getFile());
}

```

为了清晰起见，本课程中的文件 I/O 示例可能不显示异常处理，但您的代码应始终包含它。

## 可变参数

当指定标志时，几个`Files`方法接受任意数量的参数。例如，在以下方法签名中，`CopyOption`参数后的省略号表示该方法接受可变数量的参数，通常称为*可变参数*：

```java
Path Files.move(Path, Path, CopyOption...)

```

当一个方法接受可变参数时，您可以传递逗号分隔的值列表或值数组（`CopyOption[]`）。

在`move`示例中，该方法可以如下调用：

```java
import static java.nio.file.StandardCopyOption.*;

Path source = ...;
Path target = ...;
Files.move(source,
           target,
           REPLACE_EXISTING,
           ATOMIC_MOVE);

```

有关可变参数语法的更多信息，请参见可变参数。

## 原子操作

几个`Files`方法，比如`move`，可以在一些文件系统中以原子方式执行某些操作。

*原子文件操作*是一种不能被中断或“部分”执行的操作。要么整个操作被执行，要么操作失败。当您有多个进程在文件系统的同一区域上操作，并且您需要保证每个进程访问完整的文件时，这一点就很重要。

## 方法链

许多文件 I/O 方法支持*方法链*的概念。

首先调用返回对象的方法。然后立即在*该*对象上调用一个方法，该方法返回另一个对象，依此类推。许多 I/O 示例使用以下技术：

```java
String value = Charset.defaultCharset().decode(buf).toString();
UserPrincipal group =
    file.getFileSystem().getUserPrincipalLookupService().
         lookupPrincipalByName("me");

```

这种技术产生了紧凑的代码，并使您能够避免声明您不需要的临时变量。

## 什么是*通配符*？

`Files`类中有两个方法接受通配符参数，但是*通配符*是什么？

您可以使用通配符语法来指定模式匹配行为。

通配符模式被指定为一个字符串，并与其他字符串（如目录或文件名）匹配。通配符语法遵循几个简单的规则：

+   星号`*`匹配任意数量的字符（包括零个）。

+   两个星号`**`的工作方式类似于`*`，但跨越目录边界。这种语法通常用于匹配完整路径。

+   问号，`?`，匹配恰好一个字符。

+   大括号指定一组子模式。例如：

    +   `{太阳,月亮,星星}` 匹配"太阳"、"月亮"或"星星"。

    +   `{temp*,tmp*}` 匹配所有以"temp"或"tmp"开头的字符串。

+   方括号传达一组单个字符，或者当使用连字符字符（`-`）时，传达一组字符范围。例如：

    +   `[aeiou]` 匹配任何小写元音字母。

    +   `[0-9]` 匹配任何数字。

    +   `[A-Z]` 匹配任何大写字母。

    +   `[a-z,A-Z]` 匹配任何大写或小写字母。在方括号内，`*`、`?`和`\`代表它们自身。

+   所有其他字符都代表它们自身。

+   要匹配`*`、`?`或其他特殊字符，可以使用反斜杠字符`\`进行转义。例如：`\\`匹配一个反斜杠，`\?`匹配问号。

以下是一些通配符语法示例：

+   `*.html` – 匹配所有以 *.html* 结尾的字符串

+   `???` – 匹配所有恰好有三个字母或数字的字符串

+   `*[0-9]*` – 匹配所有包含数字值的字符串

+   `*.{htm,html,pdf}` – 匹配任何以 *.htm*、*.html* 或 *.pdf* 结尾的字符串

+   `a?*.java` – 匹配任何以 `a` 开头，后跟至少一个字母或数字，并以 *.java* 结尾的字符串

+   `{foo*,*[0-9]*}` – 匹配任何以 *foo* 开头的字符串或任何包含数字值的字符串

* * *

**注意：** 如果您正在键盘上输入通配符模式，并且其中包含一个特殊字符，您必须将模式放在引号中（`"*"`），使用反斜杠（`\*`），或使用命令行支持的任何转义机制。

* * *

通配符语法功能强大且易于使用。但是，如果它不满足您的需求，您也可以使用正则表达式。有关更多信息，请参阅正则表达式 课程。

有关通配符语法的更多信息，请参阅`FileSystem` 类中 `getPathMatcher` 方法的 API 规范。

## 链接感知

`Files` 类是"链接感知"的。每个 `Files` 方法都会检测在遇到符号链接时该做什么，或者提供一个选项，使您能够配置在遇到符号链接时的行为。
