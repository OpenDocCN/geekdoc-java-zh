# 异常的优点

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/advantages.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/advantages.html)

现在您知道什么是异常以及如何使用它们，是时候学习在程序中使用异常的优点了。

## 优点 1：将错误处理代码与“常规”代码分开

异常提供了一种将发生异常情况时的处理细节与程序的主要逻辑分开的手段。在传统编程中，错误检测、报告和处理经常导致令人困惑的意大利面代码。例如，考虑这里的伪代码方法，它将整个文件读入内存。

```java
readFile {
    *open the file;
    determine its size;
    allocate that much memory;
    read the file into memory;
    close the file;*
}

```

乍一看，这个函数似乎足够简单，但它忽略了所有以下潜在的错误。

+   如果文件无法打开会发生什么？

+   如果无法确定文件的长度会发生什么？

+   如果无法分配足够的内存会发生什么？

+   如果读取失败会发生什么？

+   如果文件无法关闭会发生什么？

要处理这种情况，`readFile` 函数必须有更多的代码来进行错误检测、报告和处理。以下是该函数可能的示例。

```java
errorCodeType readFile {
    initialize errorCode = 0;

    *open the file;*
    if (*theFileIsOpen*) {
        *determine the length of the file;*
        if (*gotTheFileLength*) {
            *allocate that much memory;*
            if (*gotEnoughMemory*) {
                *read the file into memory;*
                if (*readFailed*) {
                    errorCode = -1;
                }
            } else {
                errorCode = -2;
            }
        } else {
            errorCode = -3;
        }
        *close the file;*
        if (*theFileDidntClose* && *errorCode* == 0) {
            errorCode = -4;
        } else {
            errorCode = errorCode and -4;
        }
    } else {
        errorCode = -5;
    }
    return errorCode;
}

```

这里有太多的错误检测、报告和返回，原始的七行代码在混乱中丢失了。更糟糕的是，代码的逻辑流也丢失了，因此很难判断代码是否在做正确的事情：如果函数无法分配足够的内存，文件是否真的被关闭了？当您在编写三个月后修改方法时，确保代码继续执行正确的事情更加困难。许多程序员通过简单地忽略它来解决这个问题——当他们的程序崩溃时会报告错误。

异常使您能够编写代码的主要流程，并在其他地方处理异常情况。如果`readFile`函数使用异常而不是传统的错误管理技术，它会看起来更像以下内容。

```java
*readFile* {
    try {
        *open the file;
        determine its size;
        allocate that much memory;
        read the file into memory;
        close the file;*
    } catch (*fileOpenFailed*) {
       *doSomething;*
    } catch (*sizeDeterminationFailed*) {
        *doSomething;*
    } catch (*memoryAllocationFailed*) {
        *doSomething;*
    } catch (*readFailed*) {
        *doSomething;*
    } catch (*fileCloseFailed*) {
        *doSomething;*
    }
}

```

请注意，异常并不免除您进行错误检测、报告和处理的工作，但它们确实帮助您更有效地组织工作。

## 优点 2：将错误传播到调用堆栈上

异常的第二个优点是能够将错误报告传播到方法的调用堆栈上。假设`readFile`方法是主程序进行的一系列嵌套方法调用中的第四个方法：`method1`调用`method2`，`method2`调用`method3`，最终调用`readFile`。 

```java
method1 {
    *call method2;*
}

method2 {
    *call method3;*
}

method3 {
    *call readFile;*
}

```

假设`method1`是唯一对`readFile`中可能发生的错误感兴趣的方法。传统的错误通知技术会强制`method2`和`method3`将`readFile`返回的错误代码传播到调用堆栈上，直到错误代码最终到达`method1`——唯一对它们感兴趣的方法。

```java
method1 {
    errorCodeType error;
    error = *call method2;*
    if (error)
        *doErrorProcessing;*
    else
        *proceed;*
}

errorCodeType method2 {
    errorCodeType error;
    error = call method3;
    if (error)
        return error;
    else
        *proceed;*
}

errorCodeType method3 {
    errorCodeType error;
    error = call readFile;
    if (error)
        return error;
    else
        *proceed;*
}

```

请记住，Java 运行时环境会逆向搜索调用堆栈，以找到任何对处理特定异常感兴趣的方法。一个方法可以规避其内部抛出的任何异常，从而允许调用堆栈中更高层的方法捕获它。因此，只有关心错误的方法才需要担心检测错误。

```java
method1 {
    try {
        *call method2;*
    } catch (*exception* e) {
        *doErrorProcessing;*
    }
}

method2 throws *exception* {
    *call method3;*
}

method3 throws exception {
    *call readFile;*
}

```

然而，正如伪代码所示，规避异常需要中间方法付出一些努力。在方法内部可能抛出的任何已检查异常必须在其`throws`子句中指定。

## 优势 3：分组和区分错误类型

因为程序中抛出的所有异常都是对象，异常的分组或分类是类层次结构的自然结果。Java 平台中一组相关的异常类的示例是`java.io`中定义的那些  `IOException`及其后代。`IOException`是最一般的，表示在执行 I/O 时可能发生的任何类型的错误。其后代表示更具体的错误。例如，`FileNotFoundException`表示无法在磁盘上找到文件。

一个方法可以编写特定的处理程序，可以处理非常具体的异常。`FileNotFoundException`类没有后代，因此以下处理程序只能处理一种类型的异常。

```java
catch (FileNotFoundException e) {
    ...
}

```

一个方法可以通过在`catch`语句中指定任何异常的超类来基于其组或一般类型捕获异常。例如，为了捕获所有 I/O 异常，无论其具体类型如何，异常处理程序指定一个`IOException`参数。

```java
catch (IOException e) {
    ...
}

```

这个处理程序将能够捕获所有 I/O 异常，包括`FileNotFoundException`、`EOFException`等。您可以通过查询传递给异常处理程序的参数来找到发生的详细信息。例如，使用以下内容打印堆栈跟踪。

```java
catch (IOException e) {
    // Output goes to System.err.
    e.printStackTrace();
    // Send trace to stdout.
    e.printStackTrace(System.out);
}

```

你甚至可以设置一个异常处理程序，用于处理这里的任何`Exception`。

```java
// *A (too) general exception handler*
catch (Exception e) {
    ...
}

```

`Exception`类接近`Throwable`类层次结构的顶部。因此，此处理程序将捕获许多其他异常，除了处理程序打算捕获的异常之外。如果您只希望程序执行的操作是打印出用户的错误消息，然后退出，您可能希望以这种方式处理异常。

然而，在大多数情况下，您希望异常处理程序尽可能具体。原因是处理程序必须首先确定发生了什么类型的异常，然后才能决定最佳的恢复策略。实际上，通过不捕获特定错误，处理程序必须适应任何可能性。过于一般化的异常处理程序可能会使代码更容易出错，因为它会捕获和处理程序员未预料到的异常，而处理程序并不打算处理这些异常。

正如所指出的，您可以创建异常组并以一般方式处理异常，或者您可以使用特定的异常类型来区分异常并以精确方式处理异常。
