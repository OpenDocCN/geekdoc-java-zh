# 将所有内容整合在一起

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/putItTogether.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/putItTogether.html)

前面的部分描述了如何为`ListOfNumbers`类中的`writeList`方法构造`try`、`catch`和`finally`代码块。现在，让我们走一遍代码，看看可能发生什么。

当所有组件放在一起时，`writeList`方法看起来像下面这样。

```java
public void writeList() {
    PrintWriter out = null;

    try {
        System.out.println("Entering" + " try statement");

        out = new PrintWriter(new FileWriter("OutFile.txt"));
        for (int i = 0; i < SIZE; i++) {
            out.println("Value at: " + i + " = " + list.get(i));
        }
    } catch (IndexOutOfBoundsException e) {
        System.err.println("Caught IndexOutOfBoundsException: "
                           +  e.getMessage());

    } catch (IOException e) {
        System.err.println("Caught IOException: " +  e.getMessage());

    } finally {
        if (out != null) {
            System.out.println("Closing PrintWriter");
            out.close();
        } 
        else {
            System.out.println("PrintWriter not open");
        }
    }
}

```

如前所述，此方法的`try`块有三种不同的退出可能性；以下是其中两种。

1.  `try`语句中的代码失败并抛出异常。这可能是由`new FileWriter`语句引起的`IOException`，也可能是由`for`循环中错误的索引值引起的`IndexOutOfBoundsException`。

1.  一切顺利，`try`语句正常退出。

让我们看看在这两种退出可能性中`writeList`方法中会发生什么。

## 情况 1：发生异常

创建`FileWriter`的语句可能因多种原因而失败。例如，如果程序无法创建或写入指定文件，则`FileWriter`的构造函数会抛出`IOException`。

当`FileWriter`抛出`IOException`时，运行时系统立即停止执行`try`块；正在执行的方法调用不会完成。然后，运行时系统从方法调用堆栈的顶部开始搜索适当的异常处理程序。在本例中，当发生`IOException`时，`FileWriter`构造函数位于调用堆栈的顶部。然而，`FileWriter`构造函数没有适当的异常处理程序，因此运行时系统检查下一个方法——`writeList`方法——在方法调用堆栈中。`writeList`方法有两个异常处理程序：一个用于`IOException`，一个用于`IndexOutOfBoundsException`。

运行时系统按照`try`语句后出现的顺序检查`writeList`的处理程序。第一个异常处理程序的参数是`IndexOutOfBoundsException`。这与抛出的异常类型不匹配，因此运行时系统检查下一个异常处理程序——`IOException`。这与抛出的异常类型匹配，因此运行时系统结束了对适当异常处理程序的搜索。现在，运行时已找到适当的处理程序，将执行该`catch`块中的代码。

异常处理程序执行后，运行时系统将控制权传递给`finally`块。`finally`块中的代码无论上面捕获的异常如何，都会执行。在这种情况下，`FileWriter`从未被打开，也不需要关闭。`finally`块执行完毕后，程序将继续执行`finally`块后的第一条语句。

下面是当抛出`IOException`时出现的`ListOfNumbers`程序的完整输出。

```java
Entering try statement
Caught IOException: OutFile.txt
PrintWriter not open 

```

以下清单中的粗体代码显示了在这种情况下执行的语句：

```java
public void writeList() {
   PrintWriter out = null;

    try {
        System.out.println("Entering try statement");
        out = new PrintWriter(new FileWriter("OutFile.txt"));
        for (int i = 0; i < SIZE; i++)
            out.println("Value at: " + i + " = " + list.get(i));

    } catch (IndexOutOfBoundsException e) {
        System.err.println("Caught IndexOutOfBoundsException: "
                           + e.getMessage());

    } catch (IOException e) {
        System.err.println("Caught IOException: " + e.getMessage());
    } finally {
        if (out != null) {
            System.out.println("Closing PrintWriter");
            out.close();
        } 
        else {
            System.out.println("PrintWriter not open");
        }
    }
}

```

## 情景 2：try 块正常退出

在这种情况下，`try`块范围内的所有语句都成功执行且没有抛出异常。执行流程跳出`try`块，运行时系统将控制权传递给`finally`块。因为一切顺利，当控制权到达`finally`块时，`PrintWriter`是打开状态，`finally`块关闭了`PrintWriter`。同样，在`finally`块执行完毕后，程序将继续执行`finally`块后的第一条语句。

这是`ListOfNumbers`程序在没有抛出异常时的输出。

```java
Entering try statement
Closing PrintWriter

```

以下示例中的粗体代码显示了在这种情况下执行的语句。

```java
public void writeList() {
    PrintWriter out = null;
    try {
        System.out.println("Entering try statement");
        out = new PrintWriter(new FileWriter("OutFile.txt"));
        for (int i = 0; i < SIZE; i++)
            out.println("Value at: " + i + " = " + list.get(i));

    } catch (IndexOutOfBoundsException e) {
        System.err.println("Caught IndexOutOfBoundsException: "
                           + e.getMessage());

    } catch (IOException e) {
        System.err.println("Caught IOException: " + e.getMessage());

    } finally {
        if (out != null) {
            System.out.println("Closing PrintWriter");
            out.close();
        } 
        else {
            System.out.println("PrintWriter not open");
        }
    }
}

```
