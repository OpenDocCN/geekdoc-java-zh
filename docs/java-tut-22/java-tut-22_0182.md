# 复制文件或目录

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/copy.html`](https://docs.oracle.com/javase/tutorial/essential/io/copy.html)

您可以使用[`copy(Path, Path, CopyOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#copy-java.nio.file.Path-java.nio.file.Path-java.nio.file.CopyOption...-)方法复制文件或目录。如果目标文件存在，则复制将失败，除非指定了`REPLACE_EXISTING`选项。

目录可以被复制。但是，目录内的文件不会被复制，因此即使原始目录包含文件，新目录也是空的。

复制符号链接时，会复制链接的目标。如果要复制链接本身而不是链接的内容，请指定`NOFOLLOW_LINKS`或`REPLACE_EXISTING`选项。

这种方法接受可变参数。支持以下`StandardCopyOption`和`LinkOption`枚举：

+   `REPLACE_EXISTING` – 即使目标文件已经存在，也执行复制。如果目标是符号链接，则复制链接本身（而不是链接的目标）。如果目标是非空目录，则复制将失败，并显示`DirectoryNotEmptyException`异常。

+   `COPY_ATTRIBUTES` – 复制与文件关联的文件属性到目标文件。支持的确切文件属性取决于文件系统和平台，但`last-modified-time`在各个平台上都受支持，并且会被复制到目标文件。

+   `NOFOLLOW_LINKS` – 表示不应跟随符号链接。如果要复制的文件是符号链接，则复制链接本身（而不是链接的目标）。

如果您不熟悉`enums`，请参见枚举类型。

以下显示了如何使用`copy`方法：

```java
import static java.nio.file.StandardCopyOption.*;
...
Files.copy(source, target, REPLACE_EXISTING);

```

除了文件复制外，`Files`类还定义了可用于文件和流之间复制的方法。[`copy(InputStream, Path, CopyOptions...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#copy-java.io.InputStream-java.nio.file.Path-java.nio.file.CopyOption...-)方法可用于将输入流的所有字节复制到文件。[`copy(Path, OutputStream)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#copy-java.nio.file.Path-java.io.OutputStream-)方法可用于将文件的所有字节复制到输出流。

``Copy``示例使用`copy`和`Files.walkFileTree`方法支持递归复制。有关更多信息，请参见遍历文件树。
