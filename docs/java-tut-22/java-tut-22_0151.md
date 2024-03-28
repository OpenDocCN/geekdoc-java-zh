# try 块

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/try.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/try.html)

构建异常处理程序的第一步是将可能会抛出异常的代码放在一个`try`块中。一般来说，一个`try`块看起来像下面这样：

```java
try {
    *code*
}
*catch and finally blocks . . .*

```

在示例中标记为`*code*`的部分包含一个或多个可能会抛出异常的合法代码行。（`catch`和`finally`块将在接下来的两个小节中解释。）

要为`ListOfNumbers`类中的`writeList`方法构建一个异常处理程序，将`writeList`方法中可能会抛出异常的语句放在一个`try`块中。有多种方法可以做到这一点。你可以将每一行可能会抛出异常的代码放在自己的`try`块中，并为每个提供单独的异常处理程序。或者，你可以将所有的`writeList`代码放在一个单独的`try`块中，并为其关联多个处理程序。以下清单使用一个`try`块来处理整个方法，因为相关代码非常简短。

```java
private List<Integer> list;
private static final int SIZE = 10;

public void writeList() {
    PrintWriter out = null;
    try {
        System.out.println("Entered try statement");
        FileWriter f = new FileWriter("OutFile.txt");
        out = new PrintWriter(f);
        for (int i = 0; i < SIZE; i++) {
            out.println("Value at: " + i + " = " + list.get(i));
        }
    }
    catch and finally blocks  . . .
}

```

如果在`try`块中发生异常，该异常将由与之关联的异常处理程序处理。要为`try`块关联一个异常处理程序，必须在其后放置一个`catch`块；下一节，catch 块，将向你展示如何做到这一点。
