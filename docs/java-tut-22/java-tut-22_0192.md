# 其他有用的方法

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/misc.html`](https://docs.oracle.com/javase/tutorial/essential/io/misc.html)

本课程中未涵盖的一些有用方法在此处介绍。本节涵盖以下内容：

+   确定 MIME 类型

+   默认文件系统

+   路径字符串分隔符

+   文件系统的文件存储器

## 确定 MIME 类型

要确定文件的 MIME 类型，您可能会发现[`probeContentType(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#probeContentType-java.nio.file.Path-)方法很有用。例如：

```java
try {
    String type = Files.probeContentType(filename);
    if (type == null) {
        System.err.format("'%s' has an" + " unknown filetype.%n", filename);
    } else if (!type.equals("text/plain") {
        System.err.format("'%s' is not" + " a plain text file.%n", filename);
        continue;
    }
} catch (IOException x) {
    System.err.println(x);
}

```

注意，如果无法确定内容类型，`probeContentType`会返回 null。

此方法的实现高度依赖于平台，并不是绝对可靠的。内容类型由平台的默认文件类型检测器确定。例如，如果检测器根据`.class`扩展名确定文件的内容类型为`application/x-java`，可能会被欺骗。

如果默认的方法不符合您的需求，您可以提供自定义的[`FileTypeDetector`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/spi/FileTypeDetector.html)。

``电子邮件``示例使用`probeContentType`方法。

## 默认文件系统

要检索默认文件系统，请使用[`getDefault`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystems.html#getDefault--)方法。通常，此`FileSystems`方法（注意是复数形式）链接到`FileSystem`方法之一（注意是单数形式），如下所示：

```java
PathMatcher matcher =
    FileSystems.getDefault().getPathMatcher("glob:*.*");

```

## 路径字符串分隔符

POSIX 文件系统的路径分隔符是正斜杠`/`，Microsoft Windows 的路径分隔符是反斜杠`\`。其他文件系统可能使用其他分隔符。要检索默认文件系统的`Path`分隔符，可以使用以下方法之一：

```java
String separator = File.separator;
String separator = FileSystems.getDefault().getSeparator();

```

[`getSeparator`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getSeparator--)方法也用于检索任何可用文件系统的路径分隔符。

## 文件系统的文件存储器

文件系统有一个或多个文件存储器来保存其文件和目录。*文件存储器*代表底层存储设备。在 UNIX 操作系统中，每个挂载的文件系统都由一个文件存储器表示。在 Microsoft Windows 中，每个卷都由一个文件存储器表示：`C:`、`D:`等等。

要检索文件系统的所有文件存储器列表，可以使用[`getFileStores`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getFileStores--)方法。此方法返回一个`Iterable`，允许您使用增强的 for 语句遍历所有根目录。

```java
for (FileStore store: FileSystems.getDefault().getFileStores()) {
   ...
}

```

如果要检索特定文件所在的文件存储器，请使用`Files`类中的`getFileStore`方法，如下所示：

```java
Path file = ...;
FileStore store= Files.getFileStore(file);

```

`DiskUsage`示例使用`getFileStores`方法。
