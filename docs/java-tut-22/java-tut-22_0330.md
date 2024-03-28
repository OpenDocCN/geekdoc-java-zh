# 查看 JAR 文件的内容

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/view.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/view.html)

查看 JAR 文件内容的基本命令格式为：

```java
jar tf *jar-file*

```

让我们看看此命令中使用的选项和参数：

+   `t` 选项表示您想查看 JAR 文件的*目录*。

+   `f` 选项表示要查看其内容的 JAR 文件在命令行上指定。

+   `jar-file` 参数是您想要查看其内容的 JAR 文件的路径和名称。

`t` 和 `f` 选项可以以任意顺序出现，但它们之间不能有任何空格。

此命令将 JAR 文件的目录显示到 `stdout`。

您可以选择添加详细选项 `v`，以在输出中生成有关文件大小和最后修改日期的附加信息。

## 一个示例

让我们使用 Jar 工具列出我们在上一节中创建的 `TicTacToe.jar` 文件的内容：

```java
jar tf TicTacToe.jar

```

此命令将 JAR 文件的内容显示到 `stdout`：

```java
META-INF/MANIFEST.MF
TicTacToe.class
audio/
audio/beep.au
audio/ding.au
audio/return.au
audio/yahoo1.au
audio/yahoo2.au
images/
images/cross.gif
images/not.gif

```

JAR 文件包含了 `TicTacToe` 类文件以及音频和图像目录，正如预期的那样。 输出还显示 JAR 文件包含一个默认清单文件 `META-INF/MANIFEST.MF`，该文件是由 JAR 工具自动放置在存档中的。 有关更多信息，请参阅理解默认清单部分。

所有路径名都显示为正斜杠，无论您使用的是哪个平台或操作系统。 JAR 文件中的路径始终是相对的；例如，您永远不会看到以 `C:` 开头的路径。

如果使用 `v` 选项，JAR 工具将显示附加信息：

```java
jar tvf TicTacToe.jar

```

例如，TicTacToe JAR 文件的详细输出将类似于以下内容：

```java
    68 Thu Nov 01 20:00:40 PDT 2012 META-INF/MANIFEST.MF
   553 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.class
  3708 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.class
  9584 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.java
     0 Mon Sep 24 21:57:48 PDT 2012 audio/
  4032 Mon Sep 24 21:57:48 PDT 2012 audio/beep.au
  2566 Mon Sep 24 21:57:48 PDT 2012 audio/ding.au
  6558 Mon Sep 24 21:57:48 PDT 2012 audio/return.au
  7834 Mon Sep 24 21:57:48 PDT 2012 audio/yahoo1.au
  7463 Mon Sep 24 21:57:48 PDT 2012 audio/yahoo2.au
   424 Mon Sep 24 21:57:48 PDT 2012 example1.html
     0 Mon Sep 24 21:57:48 PDT 2012 images/
   157 Mon Sep 24 21:57:48 PDT 2012 images/cross.gif
   158 Mon Sep 24 21:57:48 PDT 2012 images/not.gif

```
