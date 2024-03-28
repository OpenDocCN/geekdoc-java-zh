# 提取 JAR 文件内容

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/unpack.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/unpack.html)

用于提取 JAR 文件内容的基本命令是：

```java
jar xf *jar-file [archived-file(s)]*

```

让我们看看这个命令中的选项和参数：

+   `x`选项表示您要从 JAR 存档中*提取*文件。

+   `f`选项表示要从命令行指定的 JAR *文件*中提取文件，而不是通过 stdin。

+   `jar-file`参数是要从中提取文件的 JAR 文件的文件名（或路径和文件名）。

+   `archived-file(s)`是一个可选参数，由一个以空格分隔的文件列表组成，用于从存档中提取文件。如果没有此参数，Jar 工具将提取存档中的所有文件。

通常情况下，命令中`x`和`f`选项的顺序无关紧要，但它们之间不能有空格。

在提取文件时，Jar 工具会复制所需文件并将其写入当前目录，复制文件在存档中的目录结构。原始 JAR 文件保持不变。

* * *

**注意：** 当提取文件时，Jar 工具将覆盖与提取文件具有相同路径名的任何现有文件。

* * *

## 一个示例

让我们从之前部分中使用的 TicTacToe JAR 文件中提取一些文件。回想一下，`TicTacToe.jar`的内容是：

```java
META-INF/MANIFEST.MF
TicTacToe.class
TicTacToe.class
TicTacToe.java
audio/
audio/beep.au
audio/ding.au
audio/return.au
audio/yahoo1.au
audio/yahoo2.au
example1.html
images/
images/cross.gif
images/not.gif

```

假设您想要提取`TicTacToe`类文件和`cross.gif`图像文件。为此，您可以使用以下命令：

```java
jar xf TicTacToe.jar TicTacToe.class images/cross.gif

```

这个命令做了两件事：

+   它在当前目录中放置了`TicTacToe.class`的副本。

+   如果`images`目录不存在，它会创建该目录，并在其中放置`cross.gif`的副本。

原始 TicTacToe JAR 文件保持不变。

可以以相同的方式从 JAR 文件中提取任意数量的文件。当命令没有指定要提取哪些文件时，Jar 工具将提取存档中的所有文件。例如，您可以使用以下命令提取 TicTacToe 存档中的所有文件：

```java
jar xf TicTacToe.jar

```
