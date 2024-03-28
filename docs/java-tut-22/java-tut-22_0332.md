# 更新 JAR 文件

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/update.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/update.html)

Jar 工具提供了一个`u`选项，您可以使用该选项通过修改清单或添加文件来更新现有 JAR 文件的内容。

添加文件的基本命令格式如下：

```java
jar uf *jar-file input-file(s)*

```

在此命令中：

+   `u`选项表示您要*更新*现有的 JAR 文件。

+   `f`选项表示要更新的 JAR 文件在命令行上指定。

+   `jar-file`是要更新的现有 JAR 文件。

+   `input-file(s)`是一个用空格分隔的一个或多个要添加到 JAR 文件中的文件列表。

任何已经存在于归档中且与要添加的文件具有相同路径名的文件将被覆盖。

创建新的 JAR 文件时，您可以选择使用`-C`选项来指示目录更改。有关更多信息，请参阅创建 JAR 文件部分。

## 示例

请记住，`TicTacToe.jar`包含以下内容：

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

假设你想要将文件`images/new.gif`添加到 JAR 文件中。你可以通过在`images`目录的父目录中发出以下命令来实现：

```java
jar uf TicTacToe.jar images/new.gif

```

修改后的 JAR 文件将具有以下目录：

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
images/new.gif

```

您可以在执行命令期间使用`-C`选项来“更改目录”。例如：

```java
jar uf TicTacToe.jar -C images new.gif

```

此命令将在将`new.gif`添加到 JAR 文件之前切换到`images`目录。当`new.gif`添加到归档中时，`images`目录不会包含在`new.gif`的路径名中，导致目录如下所示：

```java
META-INF/MANIFEST.MF
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
new.gif

```
