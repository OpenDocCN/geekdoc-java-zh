# 异常处理块

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/catch.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/catch.html)

通过在`try`块后直接提供一个或多个`catch`块，将异常处理程序与`try`块关联起来。在`try`块结束和第一个`catch`块开始之间不能有任何代码。

```java
try {

} catch (*ExceptionType name*) {

} catch (*ExceptionType name*) {

}

```

每个`catch`块都是一个异常处理程序，处理其参数指示的异常类型。参数类型`*ExceptionType*`声明了处理程序可以处理的异常类型，必须是从`Throwable`类继承的类名。处理程序可以使用`*name*`引用异常。

`catch`块包含在异常处理程序被调用时执行的代码。当处理程序是调用堆栈中第一个`*ExceptionType*`与抛出异常类型匹配的处理程序时，运行时系统会调用异常处理程序。如果抛出的对象可以合法地分配给异常处理程序的参数，则系统认为它匹配。

以下是`writeList`方法的两个异常处理程序：

```java
try {

} catch (IndexOutOfBoundsException e) {
    System.err.println("IndexOutOfBoundsException: " + e.getMessage());
} catch (IOException e) {
    System.err.println("Caught IOException: " + e.getMessage());
}

```

异常处理程序不仅可以打印错误消息或停止程序。它们可以进行错误恢复，提示用户做出决定，或者使用链式异常将错误传播到更高级别的处理程序，如链式异常部分所述。

## 使用一个异常处理程序捕获多种类型的异常

在 Java SE 7 及更高版本中，单个`catch`块可以处理多种类型的异常。这个特性可以减少代码重复，并减少捕获过于宽泛异常的诱惑。

在`catch`子句中，指定阻止处理的异常类型，并用竖线(`|`)分隔每种异常类型：

```java
catch (IOException|SQLException ex) {
    logger.log(ex);
    throw ex;
}

```

**注意**：如果一个`catch`块处理多种异常类型，则`catch`参数隐式为`final`。在这个例子中，`catch`参数`ex`是`final`，因此您不能在`catch`块内为其分配任何值。
