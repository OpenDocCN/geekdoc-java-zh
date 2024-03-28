# 运行 JAR 打包的软件

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/run.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/run.html)

现在您已经学会了如何创建 JAR 文件，那么您如何实际运行您打包的代码呢？考虑以下情况：

+   您的 JAR 文件包含一个应该在浏览器中运行的小程序。

+   您的 JAR 文件包含一个应该从命令行启动的应用程序。

+   您的 JAR 文件包含您想要用作扩展的代码。

本节将涵盖前两种情况。教程中关于扩展机制的单独路径涵盖了 JAR 文件作为扩展的使用。

## 打包在 JAR 文件中的小程序

要从 HTML 文件中启动任何小程序以在浏览器中运行，您可以使用`applet`标签。欲了解更多信息，请参阅 Java 小程序课程。如果小程序被打包为 JAR 文件，您唯一需要做的不同之处就是使用*archive*参数来指定 JAR 文件的相对路径。

作为示例，使用 TicTacToe 演示小程序。在显示小程序的 HTML 文件中，`applet`标签可以标记如下：

```java
<applet code=TicTacToe.class 
        width="120" height="120">
</applet>

```

如果 TicTacToe 演示被打包在名为`TicTacToe.jar`的 JAR 文件中，您可以通过添加一个`archive`参数修改`applet`标签：

```java
<applet code=TicTacToe.class 
        archive="TicTacToe.jar"
        width="120" height="120">
</applet>

```

`archive`参数指定包含`TicTacToe.class`的 JAR 文件的相对路径。在此示例中，假定 JAR 文件和 HTML 文件在同一目录中。如果它们不在同一目录中，您必须在`archive`参数的值中包含 JAR 文件的相对路径。例如，如果 JAR 文件在 HTML 文件的下一个目录中，名为`applets`的目录中，那么`applet`标签将如下所示：

```java
<applet code=TicTacToe.class 
        archive="applets/TicTacToe.jar"
        width="120" height="120">
</applet>

```

## JAR 文件作为应用程序

您可以使用 Java 启动器（`java`命令）运行 JAR 打包的应用程序。基本命令如下：

```java
java -jar *jar-file*

```

`-jar`标志告诉启动器应用程序以 JAR 文件格式打包。您只能指定一个 JAR 文件，其中必须包含所有特定于应用程序的代码。

在执行此命令之前，请确保运行时环境已经了解 JAR 文件中哪个类是应用程序的入口点。

要指示哪个类是应用程序的入口点，您必须向 JAR 文件的清单中添加一个`Main-Class`头部。该头部的格式如下：

```java
Main-Class: *classname*

```

头部的数值，`classname`，是应用程序的入口类的名称。

欲了解更多信息，请参阅设置应用程序的入口点部分。

当在清单文件中设置了`Main-Class`时，您可以从命令行运行应用程序：

```java
java -jar app.jar

```

要从位于另一个目录中的 JAR 文件运行应用程序，您必须指定该目录的路径：`java -jar path/app.jar`
