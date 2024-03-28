# Path 类

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/pathClass.html`](https://docs.oracle.com/javase/tutorial/essential/io/pathClass.html)

[`Path`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html)类是 Java SE 7 版本中引入的主要入口点之一，属于[`java.nio.file`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/package-summary.html)包。如果您的应用程序使用文件 I/O，您将希望了解此类的强大功能。

* * *

**版本说明：** 如果您有使用`java.io.File`的 JDK7 之前的代码，您仍然可以通过使用[`File.toPath`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#toPath--)方法来利用`Path`类的功能。有关更多信息，请参阅旧版文件 I/O 代码。

* * *

正如其名称所示，`Path`类是文件系统中路径的程序表示。`Path`对象包含用于构建路径的文件名和目录列表，并用于检查、定位和操作文件。

一个`Path`实例反映了底层平台。在 Solaris 操作系统中，`Path`使用 Solaris 语法（`/home/joe/foo`），而在 Microsoft Windows 中，`Path`使用 Windows 语法（`C:\home\joe\foo`）。`Path`不是系统独立的。你不能比较来自 Solaris 文件系统的`Path`并期望它与来自 Windows 文件系统的`Path`匹配，即使目录结构相同，两个实例都定位到相同的相对文件。

与`Path`对应的文件或目录可能不存在。您可以创建一个`Path`实例并以各种方式操作它：您可以附加到它，提取它的部分，将其与另一个路径进行比较。在适当的时候，您可以使用[`Files`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)类中的方法来检查与`Path`对应的文件的存在性，创建文件，打开文件，删除文件，更改其权限等。

下一页将详细讨论`Path`类。
