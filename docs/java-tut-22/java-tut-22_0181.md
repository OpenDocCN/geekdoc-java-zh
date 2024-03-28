# 删除文件或目录

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/delete.html`](https://docs.oracle.com/javase/tutorial/essential/io/delete.html)

您可以删除文件、目录或链接。对于符号链接，删除的是链接本身而不是链接的目标。对于目录，目录必须为空，否则删除操作将失败。

`Files`类提供了两种删除方法。

[`delete(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#delete-java.nio.file.Path-)方法会删除文件，如果删除失败则会抛出异常。例如，如果文件不存在，则会抛出`NoSuchFileException`异常。您可以捕获异常以确定删除失败的原因，如下所示：

```java
try {
    Files.delete(path);
} catch (NoSuchFileException x) {
    System.err.format("%s: no such" + " file or directory%n", path);
} catch (DirectoryNotEmptyException x) {
    System.err.format("%s not empty%n", path);
} catch (IOException x) {
    // File permission problems are caught here.
    System.err.println(x);
}

```

[`deleteIfExists(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#deleteIfExists-java.nio.file.Path-)方法也会删除文件，但如果文件不存在，则不会抛出异常。静默失败在您有多个线程删除文件时很有用，您不希望仅因为一个线程先执行删除操作就抛出异常。
