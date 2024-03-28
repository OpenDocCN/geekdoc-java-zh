# 遍历文件树

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/walk.html`](https://docs.oracle.com/javase/tutorial/essential/io/walk.html)

您是否需要创建一个应用程序，递归访问文件树中的所有文件？也许您需要删除树中的每个`.class`文件，或者找到在过去一年中未被访问的每个文件。您可以通过[`FileVisitor`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html)接口实现这一点。

本节涵盖以下内容：

+   FileVisitor 接口

+   启动过程

+   创建 FileVisitor 时的注意事项

+   控制流程

+   示例

## FileVisitor 接口

要遍历文件树，首先需要实现一个`FileVisitor`。`FileVisitor`指定了在遍历过程的关键点上所需的行为：当访问文件时，在访问目录之前，在访问目录之后，或者当发生故障时。该接口有四个方法对应于这些情况：

+   [`preVisitDirectory`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#preVisitDirectory-T-java.nio.file.attribute.BasicFileAttributes-) – 在访问目录条目之前调用。

+   [`postVisitDirectory`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#postVisitDirectory-T-java.io.IOException-) – 在访问目录中的所有条目之后调用。如果遇到任何错误，特定异常将传递给该方法。

+   [`visitFile`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#visitFile-T-java.nio.file.attribute.BasicFileAttributes-) – 在访问文件时调用。文件的`BasicFileAttributes`被传递给该方法，或者您可以使用 file attributes 包来读取特定的属性集。例如，您可以选择读取文件的`DosFileAttributeView`来确定文件是否设置了“hidden”位。

+   [`visitFileFailed`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#visitFileFailedy-T-java.io.IOException-) – 当无法访问文件时调用。特定异常被传递给该方法。您可以选择是否抛出异常，将其打印到控制台或日志文件等。

如果您不需要实现所有四个`FileVisitor`方法，而是扩展[`SimpleFileVisitor`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/SimpleFileVisitor.html)类，而不是实现`FileVisitor`接口。这个类实现了`FileVisitor`接口，访问树中的所有文件，并在遇到错误时抛出`IOError`。您可以扩展这个类，并仅覆盖您需要的方法。

这是一个扩展`SimpleFileVisitor`以打印文件树中所有条目的示例。它打印条目，无论条目是常规文件、符号链接、目录还是其他类型的“未指定”文件。它还打印每个文件的字节大小。遇到的任何异常都会打印到控制台。

`FileVisitor`方法以粗体显示：

```java
import static java.nio.file.FileVisitResult.*;

public static class PrintFiles
    extends SimpleFileVisitor<Path> {

    // Print information about
    // each type of file.
    @Override
    public FileVisitResult visitFile(Path file,
                                   BasicFileAttributes attr) {
        if (attr.isSymbolicLink()) {
            System.out.format("Symbolic link: %s ", file);
        } else if (attr.isRegularFile()) {
            System.out.format("Regular file: %s ", file);
        } else {
            System.out.format("Other: %s ", file);
        }
        System.out.println("(" + attr.size() + "bytes)");
        return CONTINUE;
    }

    // Print each directory visited.
    @Override
    public FileVisitResult postVisitDirectory(Path dir,
                                          IOException exc) {
        System.out.format("Directory: %s%n", dir);
        return CONTINUE;
    }

    // If there is some error accessing
    // the file, let the user know.
    // If you don't override this method
    // and an error occurs, an IOException 
    // is thrown.
    @Override
    public FileVisitResult visitFileFailed(Path file,
                                       IOException exc) {
        System.err.println(exc);
        return CONTINUE;
    }
}

```

## 启动过程

一旦您实现了您的`FileVisitor`，如何启动文件遍历？`Files`类中有两个`walkFileTree`方法。

+   [`walkFileTree(Path, FileVisitor)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#walkFileTree-java.nio.file.Path-java.nio.file.FileVisitor-)

+   [`walkFileTree(Path, Set<FileVisitOption>, int, FileVisitor)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#walkFileTree-java.nio.file.Path-java.util.Set-int-java.nio.file.FileVisitor-)

第一个方法只需要一个起始点和您的`FileVisitor`的实例。您可以按以下方式调用`PrintFiles`文件访问者：

```java
Path startingDir = ...;
PrintFiles pf = new PrintFiles();
Files.walkFileTree(startingDir, pf);

```

第二个`walkFileTree`方法还允许您额外指定访问级别的限制和一组[`FileVisitOption`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitOption.html)枚举。如果您希望确保此方法遍历整个文件树，您可以为最大深度参数指定`Integer.MAX_VALUE`。

您可以指定`FileVisitOption`枚举`FOLLOW_LINKS`，表示应该跟随符号链接。

此代码片段显示了如何调用四参数方法：

```java
import static java.nio.file.FileVisitResult.*;

Path startingDir = ...;

EnumSet<FileVisitOption> opts = EnumSet.of(FOLLOW_LINKS);

Finder finder = new Finder(pattern);
Files.walkFileTree(startingDir, opts, Integer.MAX_VALUE, finder);

```

## 创建`FileVisitor`时的注意事项

文件树以深度优先方式遍历，但不能假设子目录的访问顺序。

如果您的程序将更改文件系统，您需要仔细考虑如何实现您的`FileVisitor`。

例如，如果您正在编写递归删除，您首先删除目录中的文件，然后再删除目录本身。在这种情况下，您在`postVisitDirectory`中删除目录。

如果您正在编写递归复制，您需要在`preVisitDirectory`中创建新目录，然后尝试将文件复制到其中（在`visitFiles`中）。如果您想要保留源目录的属性（类似于 UNIX 的`cp -p`命令），您需要在文件被复制后，在`postVisitDirectory`中执行此操作。``Copy``示例展示了如何做到这一点。

如果您正在编写文件搜索，您可以在`visitFile`方法中执行比较。此方法找到所有符合您条件的文件，但不会找到目录。如果您想要找到文件和目录，您还必须在`preVisitDirectory`或`postVisitDirectory`方法中执行比较。``Find``示例展示了如何做到这一点。

你需要决定是否要遵循符号链接。例如，如果你正在删除文件，跟随符号链接可能不明智。如果你正在复制文件树，你可能希望允许它。默认情况下，`walkFileTree`不会遵循符号链接。

对于文件，会调用`visitFile`方法。如果你指定了`FOLLOW_LINKS`选项，并且你的文件树有一个指向父目录的循环链接，循环目录将在`visitFileFailed`方法中报告，带有`FileSystemLoopException`。以下代码片段显示了如何捕获循环链接，并来自于``Copy``示例：

```java
@Override
public FileVisitResult
    visitFileFailed(Path file,
        IOException exc) {
    if (exc instanceof FileSystemLoopException) {
        System.err.println("cycle detected: " + file);
    } else {
        System.err.format("Unable to copy:" + " %s: %s%n", file, exc);
    }
    return CONTINUE;
}

```

这种情况只会在程序遵循符号链接时发生。

## 控制流程

也许你想要遍历文件树查找特定目录，并且在找到后希望进程终止。也许你想要跳过特定目录。

`FileVisitor`方法返回一个[`FileVisitResult`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitResult.html)值。你可以通过在`FileVisitor`方法中返回的值来中止文件遍历过程或控制是否访问目录：

+   `CONTINUE` – 表示文件遍历应该继续。如果`preVisitDirectory`方法返回`CONTINUE`，则会访问该目录。

+   `TERMINATE` – 立即中止文件遍历。在返回此值后不会调用更多的文件遍历方法。

+   `SKIP_SUBTREE` – 当`preVisitDirectory`返回此值时，指定的目录及其子目录将被跳过。这个分支将从树中“剪掉”。

+   `SKIP_SIBLINGS` – 当`preVisitDirectory`返回此值时，指定的目录不会被访问，`postVisitDirectory`不会被调用，也不会访问更多未访问的兄弟节点。如果从`postVisitDirectory`方法返回，不会访问更多的兄弟节点。基本上，在指定的目录中不会发生更多的事情。

在这段代码片段中，任何名为`SCCS`的目录都会被跳过：

```java
import static java.nio.file.FileVisitResult.*;

public FileVisitResult
     preVisitDirectory(Path dir,
         BasicFileAttributes attrs) {
    (if (dir.getFileName().toString().equals("SCCS")) {
         return SKIP_SUBTREE;
    }
    return CONTINUE;
}

```

在这段代码片段中，一旦找到特定文件，文件名就会被打印到标准输出，并且文件遍历会终止：

```java
import static java.nio.file.FileVisitResult.*;

// The file we are looking for.
Path lookingFor = ...;

public FileVisitResult
    visitFile(Path file,
        BasicFileAttributes attr) {
    if (file.getFileName().equals(lookingFor)) {
        System.out.println("Located file: " + file);
        return TERMINATE;
    }
    return CONTINUE;
}

```

## 示例

以下示例演示了文件遍历机制：

+   ``Find`` – 递归查找符合特定通配符模式的文件和目录。此示例在查找文件中讨论。

+   ``Chmod`` – 递归更改文件树上的权限（仅适用于 POSIX 系统）。

+   ``Copy`` – 递归复制文件树。

+   ``WatchDir`` – 演示了监视目录中已创建、删除或修改的文件的机制。使用`-r`选项调用此程序会监视整个树的更改。有关文件通知服务的更多信息，请参见监视目录的更改。
