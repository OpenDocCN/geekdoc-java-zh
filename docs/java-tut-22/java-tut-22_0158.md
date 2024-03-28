# 链式异常

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/chained.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/chained.html)

应用程序通常通过抛出另一个异常来响应异常。实际上，第一个异常*导致*第二个异常。知道一个异常导致另一个异常时会非常有帮助。*链式异常*帮助程序员做到这一点。

以下是支持链式异常的`Throwable`中的方法和构造函数。

```java
Throwable getCause()
Throwable initCause(Throwable)
Throwable(String, Throwable)
Throwable(Throwable)

```

`initCause`和`Throwable`构造函数的`Throwable`参数是导致当前异常的异常。`getCause`返回导致当前异常的异常，`initCause`设置当前异常的原因。

以下示例展示了如何使用链式异常。

```java
try {

} catch (IOException e) {
    throw new SampleException("Other IOException", e);
}

```

在这个例子中，当捕获到`IOException`时，会创建一个带有原始原因附加的新的`SampleException`异常，并将异常链抛到下一个更高级别的异常处理程序。

## 访问堆栈跟踪信息

现在假设更高级别的异常处理程序想要以自己的格式转储堆栈跟踪。

* * *

**定义：** *堆栈跟踪*提供了关于当前线程执行历史的信息，并列出了在异常发生时调用的类和方法的名称。堆栈跟踪是一个有用的调试工具，当抛出异常时，通常会利用它。

* * *

以下代码展示了如何在异常对象上调用`getStackTrace`方法。

```java
catch (Exception cause) {
    StackTraceElement elements[] = cause.getStackTrace();
    for (int i = 0, n = elements.length; i < n; i++) {       
        System.err.println(elements[i].getFileName()
            + ":" + elements[i].getLineNumber() 
            + ">> "
            + elements[i].getMethodName() + "()");
    }
}

```

## 日志记录 API

下一个代码片段记录了异常发生的位置，位于`catch`块内部。然而，它不是手动解析堆栈跟踪并将输出发送到`System.err()`，而是使用[`java.util.logging`](https://docs.oracle.com/javase/8/docs/api/java/util/logging/package-summary.html)包中的日志记录功能将输出发送到文件。

```java
try {
    Handler handler = new FileHandler("OutFile.log");
    Logger.getLogger("").addHandler(handler);

} catch (IOException e) {
    Logger logger = Logger.getLogger("*package.name*"); 
    StackTraceElement elements[] = e.getStackTrace();
    for (int i = 0, n = elements.length; i < n; i++) {
        logger.log(Level.WARNING, elements[i].getMethodName());
    }
}

```
