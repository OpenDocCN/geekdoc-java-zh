# 创建和读取目录

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/dirs.html`](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html)

其中一些先前讨论过的方法，如`delete`，适用于文件、链接*和*目录。但是如何列出文件系统顶部的所有目录？如何列出目录的内容或创建目录？

本节涵盖了以下特定于目录的功能：

+   列出文件系统的根目录

+   创建目录

+   创建临时目录

+   列出目录的内容

+   通过使用 Globbing 筛选目录列表

+   编写自己的目录过滤器

## 列出文件系统的根目录

您可以使用[`FileSystem.getRootDirectories`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getRootDirectories--)方法列出文件系统的所有根目录。此方法返回一个`Iterable`，使您可以使用增强型 for 语句遍历所有根目录。

以下代码片段打印默认文件系统的根目录：

```java
Iterable<Path> dirs = FileSystems.getDefault().getRootDirectories();
for (Path name: dirs) {
    System.err.println(name);
}

```

## 创建目录

您可以使用[`createDirectory(Path, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createDirectory-java.nio.file.Path-java.nio.file.attribute.FileAttribute...-)方法创建一个新目录。如果不指定任何`FileAttributes`，新目录将具有默认属性。例如：

```java
Path dir = ...;
Files.createDirectory(path);

```

以下代码片段在具有特定权限的 POSIX 文件系统上创建一个新目录：

```java
Set<PosixFilePermission> perms =
    PosixFilePermissions.fromString("rwxr-x---");
FileAttribute<Set<PosixFilePermission>> attr =
    PosixFilePermissions.asFileAttribute(perms);
Files.createDirectory(file, attr);

```

要在可能尚不存在一个或多个父目录的情况下创建几层深的目录，您可以使用方便的方法[`createDirectories(Path, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createDirectories-java.nio.file.Path-java.nio.file.attribute.FileAttribute...-)。与`createDirectory(Path, FileAttribute<?>)`方法一样，您可以指定一组可选的初始文件属性。以下代码片段使用默认属性：

```java
Files.createDirectories(Paths.get("foo/bar/test"));

```

目录是按需自上而下创建的。在`foo/bar/test`示例中，如果`foo`目录不存在，则会创建它。接下来，如果需要，将创建`bar`目录，最后创建`test`目录。

在创建一些但不是所有父目录后，此方法可能会失败。

## 创建临时目录

你可以使用`createTempDirectory`方法之一创建临时目录：

+   [`createTempDirectory(Path, String, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createTempDirectory-java.nio.file.Path-java.lang.String-java.nio.file.attribute.FileAttribute...-)

+   [`createTempDirectory(String, FileAttribute<?>...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createTempDirectory-java.lang.String-java.nio.file.attribute.FileAttribute...-)

第一个方法允许代码指定临时目录的位置，第二个方法在默认临时文件目录中创建一个新目录。

## 列出目录的内容

您可以使用[`newDirectoryStream(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newDirectoryStream-java.nio.file.Path-)方法列出目录的所有内容。此方法返回一个实现了[`DirectoryStream`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/DirectoryStream.html)接口的对象。实现`DirectoryStream`接口的类还实现了`Iterable`，因此您可以遍历目录流，读取所有对象。这种方法适用于非常大的目录。

* * *

**记住：** 返回的`DirectoryStream`是一个*流*。如果你没有使用`try-`with-resources 语句，请不要忘记在`finally`块中关闭流。`try-`with-resources 语句会为您处理这个问题。

* * *

以下代码片段展示了如何打印目录的内容：

```java
Path dir = ...;
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir)) {
    for (Path file: stream) {
        System.out.println(file.getFileName());
    }
} catch (IOException | DirectoryIteratorException x) {
    // IOException can never be thrown by the iteration.
    // In this snippet, it can only be thrown by newDirectoryStream.
    System.err.println(x);
}

```

迭代器返回的`Path`对象是相对于目录解析的条目名称。因此，如果您正在列出`/tmp`目录的内容，则条目将以`/tmp/a`、`/tmp/b`等形式返回。

此方法返回目录的全部内容：文件、链接、子目录和隐藏文件。如果您想更有选择地检索内容，可以使用本页后面描述的其他`newDirectoryStream`方法之一。

请注意，如果在目录迭代过程中出现异常，则会抛出`DirectoryIteratorException`，其原因是`IOException`。迭代器方法不能抛出异常。

## 通过使用 Globbing 筛选目录列表

如果您只想获取文件和子目录，其中每个名称都匹配特定模式，可以使用[`newDirectoryStream(Path, String)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newDirectoryStream-java.nio.file.Path-java.lang.String-)方法，该方法提供了内置的 glob 过滤器。如果您不熟悉 glob 语法，请参阅什么是 Glob？

例如，以下代码片段列出了与 Java 相关的文件：*.class*，*.java*和*.jar*文件。

```java
Path dir = ...;
try (DirectoryStream<Path> stream =
     Files.newDirectoryStream(dir, "*.{java,class,jar}")) {
    for (Path entry: stream) {
        System.out.println(entry.getFileName());
    }
} catch (IOException x) {
    // IOException can never be thrown by the iteration.
    // In this snippet, it can // only be thrown by newDirectoryStream.
    System.err.println(x);
}

```

## 编写自己的目录过滤器

也许您想要根据除了模式匹配之外的某些条件来过滤目录的内容。您可以通过实现[`DirectoryStream.Filter<T>`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/DirectoryStream.Filter.html)接口来创建自己的过滤器。该接口由一个方法`accept`组成，用于确定文件是否满足搜索要求。

例如，以下代码片段实现了一个仅检索目录的过滤器：

```java
DirectoryStream.Filter<Path> filter =
    newDirectoryStream.Filter<Path>() {
    public boolean accept(Path file) throws IOException {
        try {
            return (Files.isDirectory(path));
        } catch (IOException x) {
            // Failed to determine if it's a directory.
            System.err.println(x);
            return false;
        }
    }
};

```

一旦过滤器被创建，就可以通过使用[`newDirectoryStream(Path, DirectoryStream.Filter<? super Path>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newDirectoryStream-java.nio.file.Path-java.nio.file.DirectoryStream.Filter-)方法来调用它。以下代码片段使用`isDirectory`过滤器仅将目录的子目录打印到标准输出：

```java
Path dir = ...;
try (DirectoryStream<Path>
                       stream = Files.newDirectoryStream(dir, filter)) {
    for (Path entry: stream) {
        System.out.println(entry.getFileName());
    }
} catch (IOException x) {
    System.err.println(x);
}

```

此方法仅用于过滤单个目录。然而，如果您想要在文件树中找到所有子目录，您将使用遍历文件树的机制。
