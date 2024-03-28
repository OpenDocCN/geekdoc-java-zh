# 命令行参数

> 原文：[`docs.oracle.com/javase/tutorial/essential/environment/cmdLineArgs.html`](https://docs.oracle.com/javase/tutorial/essential/environment/cmdLineArgs.html)

一个 Java 应用程序可以从命令行接受任意数量的参数。这允许用户在启动应用程序时指定配置信息。

用户在调用应用程序时输入命令行参数，并在要运行的类名后指定这些参数。例如，假设一个名为`Sort`的 Java 应用程序对文件中的行进行排序。要对名为`friends.txt`的文件中的数据进行排序，用户会输入：

```java
java Sort friends.txt

```

当一个应用程序启动时，运行时系统会通过一个`String`数组将命令行参数传递给应用程序的主方法。在前面的示例中，传递给`Sort`应用程序的命令行参数是包含一个单独`String`的数组：`"friends.txt"`。

## 回显命令行参数

``Echo``示例会将每个命令行参数单独显示在一行上：

```java

public class Echo {
    public static void main (String[] args) {
        for (String s: args) {
            System.out.println(s);
        }
    }
}

```

以下示例展示了用户如何运行`Echo`。用户输入以斜体显示。

```java
*java Echo Drink Hot Java*
Drink
Hot
Java

```

请注意，应用程序会将每个单词  `Drink`、`Hot` 和 `Java`  单独显示在一行上。这是因为空格字符分隔命令行参数。要将`Drink`、`Hot`和`Java`解释为单个参数，用户应该用引号将它们括起来。

```java
*java Echo "Drink Hot Java"*
Drink Hot Java

```

## 解析数值型命令行参数

如果一个应用程序需要支持一个表示数字的命令行参数，它必须将代表数字的`String`参数（如"34"）转换为数值。以下是一个将命令行参数转换为`int`的代码片段：

```java
int firstArg;
if (args.length > 0) {
    try {
        firstArg = Integer.parseInt(args[0]);
    } catch (NumberFormatException e) {
        System.err.println("Argument" + args[0] + " must be an integer.");
        System.exit(1);
    }
}

```

如果`parseInt`的`args[0]`的格式无效，会抛出`NumberFormatException`。所有的`Number`类  `Integer`、`Float`、`Double`等等  都有`parseXXX`方法，将代表数字的`String`转换为其类型的对象。
