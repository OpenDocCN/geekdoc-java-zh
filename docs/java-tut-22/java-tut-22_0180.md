# 检查文件或目录

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/check.html`](https://docs.oracle.com/javase/tutorial/essential/io/check.html)

您有一个代表文件或目录的`Path`实例，但该文件是否存在于文件系统中？它是否可读？可写？可执行？

## 验证文件或目录的存在

`Path`类中的方法是语法的，意味着它们在`Path`实例上操作。但最终您必须访问文件系统来验证特定的`Path`是否存在或不存在。您可以使用[`exists(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#exists-java.nio.file.Path-java.nio.file.LinkOption...-)和[`notExists(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#notExists-java.nio.file.Path-java.nio.file.LinkOption...-)方法。请注意，`!Files.exists(path)`并不等同于`Files.notExists(path)`。当您测试文件的存在时，可能有三种结果：

+   验证文件存在。

+   验证文件不存在。

+   文件的状态未知。当程序无法访问文件时，会出现此结果。

如果`exists`和`notExists`都返回`false`，则无法验证文件的存在。

## 检查文件的可访问性

要验证程序是否可以按需访问文件，可以使用[`isReadable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isReadable-java.nio.file.Path-)、[`isWritable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isWritable-java.nio.file.Path-)和[`isExecutable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isExecutable-java.nio.file.Path-)方法。

以下代码片段验证特定文件是否存在，并且程序是否有执行文件的能力。

```java
Path file = ...;
boolean isRegularExecutableFile = Files.isRegularFile(file) &
         Files.isReadable(file) & Files.isExecutable(file);

```

* * *

**注意：** 一旦其中任何方法完成，就不能保证可以访问文件。许多应用程序中的常见安全漏洞是执行检查然后访问文件。要获取更多信息，请使用您喜欢的搜索引擎查找`TOCTTOU`（发音为*TOCK-too*）。

* * *

## 检查两个路径是否定位到相同文件

当您有一个使用符号链接的文件系统时，可能会有两个不同的路径定位到同一个文件。[`isSameFile(Path, Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isSameFile-java.nio.file.Path-java.nio.file.Path-)方法比较两个路径，以确定它们是否在文件系统上定位到同一个文件。例如：

```java
Path p1 = ...;
Path p2 = ...;

if (Files.isSameFile(p1, p2)) {
    // Logic when the paths locate the same file
}

```
