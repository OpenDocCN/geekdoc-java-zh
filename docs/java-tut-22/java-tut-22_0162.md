# 摘要

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/summary.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/summary.html)

程序可以使用异常来指示发生了错误。要抛出异常，请使用`throw`语句，并提供一个异常对象  一个`Throwable`的后代  以提供有关发生的具体错误的信息。抛出未捕获的已检查异常的方法必须在其声明中包含一个`throws`子句。

程序可以通过使用`try`、`catch`和`finally`块的组合来捕获异常。

+   `try`块标识出可能发生异常的代码块。

+   `catch`块标识出一个代码块，称为异常处理程序，可以处理特定类型的异常。

+   `finally`块标识出保证执行的代码块，并且是关闭文件、恢复资源以及在`try`块中封闭的代码之后进行清理的正确位置。

`try`语句应至少包含一个`catch`块或一个`finally`块，并且可以有多个`catch`块。

异常对象的类表示抛出的异常类型。异常对象可以包含有关错误的进一步信息，包括错误消息。通过异常链接，一个异常可以指向导致它的异常，后者又可以指向导致*它*的异常，依此类推。
