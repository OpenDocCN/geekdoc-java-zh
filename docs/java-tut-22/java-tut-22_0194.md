# 摘要

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/summary.html`](https://docs.oracle.com/javase/tutorial/essential/io/summary.html)

`java.io` 包包含许多类，您的程序可以使用这些类来读取和写入数据。大多数类实现顺序访问流。顺序访问流可以分为两组：那些读取和写入字节的流以及读取和写入 Unicode 字符的流。每个顺序访问流都有其特长，例如从文件中读取或写入数据，过滤读取或写入的数据，或者序列化对象。

`java.nio.file` 包提供了广泛的文件和文件系统 I/O 支持。这是一个非常全面的 API，但关键入口点如下：

+   `Path` 类具有操作路径的方法。

+   `Files` 类具有文件操作的方法，例如移动、复制、删除，以及检索和设置文件属性的方法。

+   `FileSystem` 类具有各种方法用于获取有关文件系统的信息。

关于 NIO.2 的更多信息可以在 [OpenJDK: NIO](http://openjdk.java.net/projects/nio/) 项目网站上找到。该网站包括 NIO.2 提供的超出本教程范围的功能资源，例如多播、异步 I/O 和创建自己的文件系统实现。
