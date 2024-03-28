# 查找文件

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/find.html`](https://docs.oracle.com/javase/tutorial/essential/io/find.html)

如果你曾经使用过 shell 脚本，你很可能使用过模式匹配来定位文件。事实上，你可能已经广泛使用了它。如果你还没有使用过，模式匹配使用特殊字符创建模式，然后文件名可以与该模式进行比较。例如，在大多数 shell 脚本中，星号，`*`，匹配任意数量的字符。例如，以下命令列出当前目录中以`.html`结尾的所有文件：

```java
% ls *.html

```

`java.nio.file`包为这一有用功能提供了编程支持。每个文件系统实现都提供了一个[`PathMatcher`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/PathMatcher.html)。你可以通过在`FileSystem`类中使用[`getPathMatcher(String)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getPathMatcher-java.lang.String-)方法来检索文件系统的`PathMatcher`。以下代码片段获取默认文件系统的路径匹配器：

```java
String pattern = ...;
PathMatcher matcher =
    FileSystems.getDefault().getPathMatcher("glob:" + pattern);

```

传递给`getPathMatcher`的字符串参数指定语法风格和要匹配的模式。本示例指定了*glob*语法。如果你不熟悉 glob 语法，请参阅什么是 Glob。

Glob 语法易于使用和灵活，但如果你喜欢，也可以使用正则表达式，或*regex*语法。有关正则表达式的更多信息，请参阅正则表达式课程。一些文件系统实现可能支持其他语法。

如果你想使用其他形式的基于字符串的模式匹配，你可以创建自己的`PathMatcher`类。本页中的示例使用 glob 语法。

一旦你创建了`PathMatcher`实例，你就可以准备好根据它匹配文件。`PathMatcher`接口有一个方法，[`matches`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/PathMatcher.html#matches-java.nio.file.Path-)，它接受一个`Path`参数并返回一个布尔值：它要么匹配模式，要么不匹配。以下代码片段查找以`.java`或`.class`结尾的文件并将这些文件打印到标准输出：

```java
PathMatcher matcher =
    FileSystems.getDefault().getPathMatcher("glob:*.{java,class}");

Path filename = ...;
if (matcher.matches(filename)) {
    System.out.println(filename);
}

```

## 递归模式匹配

搜索与特定模式匹配的文件与遍历文件树密切相关。有多少次你知道一个文件*在某处*在文件系统上，但在哪里？或者也许你需要找到文件树中具有特定文件扩展名的所有文件。

`Find`示例正是如此。`Find`类似于 UNIX 的`find`实用程序，但功能更简化。你可以扩展这个示例以包含其他功能。例如，`find`实用程序支持`-prune`标志来排除搜索中的整个子树。你可以通过在`preVisitDirectory`方法中返回`SKIP_SUBTREE`来实现该功能。要实现`-L`选项，即跟随符号链接，你可以使用四个参数的`walkFileTree`方法，并传入`FOLLOW_LINKS`枚举（但请确保在`visitFile`方法中测试循环链接）。

要运行 Find 应用程序，请使用以下格式：

```java
% java Find <path> -name "<glob_pattern>"

```

模式被放置在引号内，以防止 shell 解释任何通配符。例如：

```java
% java Find . -name "*.html"

```

这里是`Find`示例的源代码：

```java
/**
 * Sample code that finds files that match the specified glob pattern.
 * For more information on what constitutes a glob pattern, see
 * https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob
 *
 * The file or directories that match the pattern are printed to
 * standard out.  The number of matches is also printed.
 *
 * When executing this application, you must put the glob pattern
 * in quotes, so the shell will not expand any wild cards:
 *              java Find . -name "*.java"
 */

import java.io.*;
import java.nio.file.*;
import java.nio.file.attribute.*;
import static java.nio.file.FileVisitResult.*;
import static java.nio.file.FileVisitOption.*;
import java.util.*;

public class Find {

    public static class Finder
        extends SimpleFileVisitor<Path> {

        private final PathMatcher matcher;
        private int numMatches = 0;

        Finder(String pattern) {
            matcher = FileSystems.getDefault()
                    .getPathMatcher("glob:" + pattern);
        }

        // Compares the glob pattern against
        // the file or directory name.
        void find(Path file) {
            Path name = file.getFileName();
            if (name != null && matcher.matches(name)) {
                numMatches++;
                System.out.println(file);
            }
        }

        // Prints the total number of
        // matches to standard out.
        void done() {
            System.out.println("Matched: "
                + numMatches);
        }

        // Invoke the pattern matching
        // method on each file.
        @Override
        public FileVisitResult visitFile(Path file,
                BasicFileAttributes attrs) {
            find(file);
            return CONTINUE;
        }

        // Invoke the pattern matching
        // method on each directory.
        @Override
        public FileVisitResult preVisitDirectory(Path dir,
                BasicFileAttributes attrs) {
            find(dir);
            return CONTINUE;
        }

        @Override
        public FileVisitResult visitFileFailed(Path file,
                IOException exc) {
            System.err.println(exc);
            return CONTINUE;
        }
    }

    static void usage() {
        System.err.println("java Find <path>" +
            " -name \"<glob_pattern>\"");
        System.exit(-1);
    }

    public static void main(String[] args)
        throws IOException {

        if (args.length < 3 || !args[1].equals("-name"))
            usage();

        Path startingDir = Paths.get(args[0]);
        String pattern = args[2];

        Finder finder = new Finder(pattern);
        Files.walkFileTree(startingDir, finder);
        finder.done();
    }
}

```

递归遍历文件树的内容在遍历文件树中有详细介绍。
