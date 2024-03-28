# 文件 I/O（具有 NIO.2 功能）

> 译文：[`docs.oracle.com/javase/tutorial/essential/io/fileio.html`](https://docs.oracle.com/javase/tutorial/essential/io/fileio.html)

* * *

**注意：** 本教程反映了 JDK 7 版本中引入的文件 I/O 机制。Java SE 6 版本的文件 I/O 教程很简短，但您可以下载包含早期文件 I/O 内容的[Java SE Tutorial 2008-03-14](http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-tutorials-419421.html#tutorial-2008_03_14-oth-JPR)版本的教程。

* * *

`java.nio.file`包及其相关包`java.nio.file.attribute`为文件 I/O 和访问默认文件系统提供了全面支持。尽管 API 有许多类，但您只需关注其中的一些入口点。您会发现这个 API 非常直观和易于使用。

本教程首先询问什么是路径？然后介绍了该包的主要入口点 Path 类。解释了与语法操作相关的`Path`类中的方法。然后教程转向包中的另一个主要类`Files`类，其中包含处理文件操作的方法。首先介绍了许多 file operations 共有的一些概念。然后介绍了用于检查、删除、复制和移动文件的方法。

教程展示了在继续学习 file I/O 和 directory I/O 之前如何管理元数据。解释了随机访问文件并检查了与符号链接和硬链接相关的问题。

接下来，涵盖了一些非常强大但更高级的主题。首先演示了递归遍历文件树的能力，然后介绍了如何使用通配符搜索文件的信息。接下来，解释并演示了如何监视目录以进行更改。然后，给出了一些其他地方不适用的方法的关注。

最后，如果您在 Java SE 7 发布之前编写了文件 I/O 代码，有一个从旧 API 到新 API 的映射，以及关于`File.toPath`方法的重要信息，供希望利用新 API 而无需重写现有代码的开发人员参考。
