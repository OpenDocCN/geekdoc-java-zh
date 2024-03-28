# 指定方法抛出的异常

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/declaring.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/declaring.html)

前一节展示了如何为`ListOfNumbers`类中的`writeList`方法编写异常处理程序。有时，代码捕获可能发生的异常是合适的。然而，在其他情况下，最好让调用堆栈中更高层的方法处理异常。例如，如果你将`ListOfNumbers`类作为一个类包的一部分提供，你可能无法预料到包的所有用户的需求。在这种情况下，最好*不要*捕获异常，而是让调用堆栈中更高层的方法来处理它。

如果`writeList`方法不捕获其中可能发生的已检查异常，那么`writeList`方法必须指定它可以抛出这些异常。让我们修改原始的`writeList`方法，以指定它可以抛出的异常，而不是捕获它们。为了提醒你，这里是原始版本的`writeList`方法，它不会编译通过。

```java
public void writeList() {
    PrintWriter out = new PrintWriter(new FileWriter("OutFile.txt"));
    for (int i = 0; i < SIZE; i++) {
        out.println("Value at: " + i + " = " + list.get(i));
    }
    out.close();
}

```

要指定`writeList`可能会抛出两个异常，需要在`writeList`方法的方法声明中添加一个`throws`子句。`throws`子句包括`throws`关键字，后面跟着一个逗号分隔的由该方法抛出的所有异常的列表。该子句放在方法名和参数列表之后，方法范围定义的大括号之前；以下是一个示例。

```java
public void writeList() throws IOException, IndexOutOfBoundsException {

```

请记住`IndexOutOfBoundsException`是一个未经检查的异常；在`throws`子句中包含它并不是强制性的。你可以只写如下内容。

```java
public void writeList() throws IOException {

```
