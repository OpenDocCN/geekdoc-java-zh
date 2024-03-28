# 移动文件或目录

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/move.html`](https://docs.oracle.com/javase/tutorial/essential/io/move.html)

您可以使用[`move(Path, Path, CopyOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#move-java.nio.file.Path-java.nio.file.Path-java.nio.file.CopyOption...-)方法移动文件或目录。如果目标文件存在，则移动失败，除非指定了`REPLACE_EXISTING`选项。

空目录可以被移动。如果目录不为空，只有当可以移动目录而不移动该目录的内容时才允许移动。在 UNIX 系统上，将目录移动到同一分区通常是重命名目录。在这种情况下，即使目录包含文件，此方法也有效。

此方法接受可变参数 – 支持以下`StandardCopyOption`枚举：

+   `REPLACE_EXISTING` – 即使目标文件已经存在，也执行移动操作。如果目标是一个符号链接，符号链接会被替换，但它指向的内容不受影响。

+   `ATOMIC_MOVE` – 将移动操作作为原子文件操作执行。如果文件系统不支持原子移动，则会抛出异常。使用`ATOMIC_MOVE`，您可以将文件移动到目录中，并确保任何监视目录的进程访问完整文件。

以下展示了如何使用`move`方法：

```java
import static java.nio.file.StandardCopyOption.*;
...
Files.move(source, target, REPLACE_EXISTING);

```

尽管可以像所示在单个目录上实现`move`方法，但该方法通常与文件树递归机制一起使用。有关更多信息，请参见遍历文件树。
