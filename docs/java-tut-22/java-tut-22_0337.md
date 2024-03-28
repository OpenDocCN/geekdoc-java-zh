# 设置应用程序的入口点

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/appman.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/appman.html)

如果你有一个打包在 JAR 文件中的应用程序，你需要一种方式来指示 JAR 文件中哪个类是你的应用程序的入口点。你可以通过清单中的`Main-Class`头部提供这些信息，其一般形式为：

```java
Main-Class: *classname*

```

值*`classname`*是你的应用程序的入口点类的名称。

请记住，入口点是一个具有签名为`public static void main(String[] args)`的方法的类。

在清单中设置了`Main-Class`头部后，你可以使用以下形式的`java`命令运行 JAR 文件：

```java
java -jar *JAR-name*

```

在`Main-Class`头部指定的类的`main`方法将被执行。

## 一个示例

当我们运行 JAR 文件时，我们希望在包`MyPackage`中的类`MyClass`中执行`main`方法。

我们首先创建一个名为`Manifest.txt`的文本文件，内容如下：

```java
Main-Class: MyPackage.MyClass

```

* * *

**警告：** 文本文件必须以换行符或回车符结尾。如果最后一行没有以换行符或回车符结尾，它将无法正确解析。

* * *

然后我们通过输入以下命令创建一个名为`MyJar.jar`的 JAR 文件：

```java
jar cfm MyJar.jar Manifest.txt MyPackage/*.class

```

这将创建带有以下内容的清单的 JAR 文件：

```java
Manifest-Version: 1.0
Created-By: 1.7.0_06 (Oracle Corporation)
Main-Class: MyPackage.MyClass

```

当你使用以下命令运行 JAR 文件时，`MyClass`的`main`方法将被执行：

```java
java -jar MyJar.jar

```

## 使用 JAR 工具设置入口点

'e'标志（代表'entrypoint'）创建或覆盖清单中的`Main-Class`属性。它可用于创建或更新 JAR 文件。使用它来指定应用程序的入口点，而无需编辑或创建清单文件。

例如，这个命令创建了`app.jar`，其中清单中的`Main-Class`属性值设置为`MyApp`：

```java
jar cfe app.jar MyApp MyApp.class

```

你可以通过运行以下命令直接调用这个应用程序：

```java
java -jar app.jar

```

如果入口类名在一个包中，它可能使用'.'（点）字符作为分隔符。例如，如果`Main.class`在一个名为`foo`的包中，入口点可以用以下方式指定：

```java
jar cfe Main.jar foo.Main foo/Main.class

```
