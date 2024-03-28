# 链接，符号或其他

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/links.html`](https://docs.oracle.com/javase/tutorial/essential/io/links.html)

如前所述，`java.nio.file`包，特别是`Path`类是“链接感知”的。每个`Path`方法都会检测遇到符号链接时该做什么，或者提供一个选项，使您能够配置遇到符号链接时的行为。

到目前为止的讨论一直是关于符号或*软*链接，但一些文件系统也支持硬链接。*硬链接*比符号链接更受限制，具体如下：

+   链接的目标必须存在。

+   通常不允许在目录上创建硬链接。

+   硬链接不允许跨分区或卷。因此，它们不能存在于不同文件系统之间。

+   一个硬链接看起来和行为都像一个普通文件，所以它们可能很难找到。

+   从所有方面来看，硬链接与原始文件是相同的实体。它们具有相同的文件权限、时间戳等。所有属性都是相同的。

由于这些限制，硬链接不像符号链接那样经常使用，但`Path`方法与硬链接无缝配合。

有几种方法专门处理链接，并在以下部分中介绍：

+   创建符号链接

+   创建硬链接

+   检测符号链接

+   查找链接的目标

## 创建符号链接

如果你的文件系统支持，你可以使用[`createSymbolicLink(Path, Path, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createSymbolicLink-java.nio.file.Path-java.nio.file.Path-java.nio.file.attribute.FileAttribute...-)方法创建一个符号链接。第二个`Path`参数表示目标文件或目录，可能存在也可能不存在。以下代码片段创建了一个带有默认权限的符号链接：

```java
Path newLink = ...;
Path target = ...;
try {
    Files.createSymbolicLink(newLink, target);
} catch (IOException x) {
    System.err.println(x);
} catch (UnsupportedOperationException x) {
    // Some file systems do not support symbolic links.
    System.err.println(x);
}

```

`FileAttributes` vararg 使您能够指定在创建链接时原子设置的初始文件属性。但是，这个参数是为将来使用而设计的，目前尚未实现。

## 创建硬链接

你可以使用[`createLink(Path, Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createLink-java.nio.file.Path-java.nio.file.Path-)方法创建一个到现有文件的硬（或*常规*）链接。第二个`Path`参数定位现有文件，它必须存在，否则会抛出`NoSuchFileException`。以下代码片段展示了如何创建链接：

```java
Path newLink = ...;
Path existingFile = ...;
try {
    Files.createLink(newLink, existingFile);
} catch (IOException x) {
    System.err.println(x);
} catch (UnsupportedOperationException x) {
    // Some file systems do not
    // support adding an existing
    // file to a directory.
    System.err.println(x);
}

```

## 检测符号链接

要确定`Path`实例是否是符号链接，可以使用[`isSymbolicLink(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isSymbolicLink-java.nio.file.Path-)方法。以下代码片段展示了如何：

```java
Path file = ...;
boolean isSymbolicLink =
    Files.isSymbolicLink(file);

```

欲了解更多信息，请参阅管理元数据。

## 查找链接的目标

通过使用[`readSymbolicLink(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readSymbolicLink-java.nio.file.Path-)方法，您可以获取符号链接的目标，如下所示：

```java
Path link = ...;
try {
    System.out.format("Target of link" +
        " '%s' is '%s'%n", link,
        Files.readSymbolicLink(link));
} catch (IOException x) {
    System.err.println(x);
}

```

如果`Path`不是一个符号链接，该方法会抛出`NotLinkException`。
