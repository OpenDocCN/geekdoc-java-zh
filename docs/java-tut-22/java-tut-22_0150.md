# 捕获和处理异常

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/handling.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/handling.html)

本节描述了如何使用三个异常处理组件——`try`、`catch`和`finally`块——编写异常处理程序。然后，解释了在 Java SE 7 中引入的`try-`with-resources 语句。`try-`with-resources 语句特别适用于使用`Closeable`资源的情况，比如流。

本节的最后部分通过一个示例演示并分析了各种情况下的发生情况。

以下示例定义并实现了一个名为`ListOfNumbers`的类。在构造时，`ListOfNumbers`创建一个包含 10 个顺序值为 0 到 9 的`Integer`元素的`ArrayList`。`ListOfNumbers`类还定义了一个名为`writeList`的方法，该方法将数字列表写入名为`OutFile.txt`的文本文件。此示例使用了`java.io`中定义的输出类，这些类在基本 I/O 中有介绍。

```java
// Note: This class will not compile yet.
import java.io.*;
import java.util.List;
import java.util.ArrayList;

public class ListOfNumbers {

    private List<Integer> list;
    private static final int SIZE = 10;

    public ListOfNumbers () {
        list = new ArrayList<Integer>(SIZE);
        for (int i = 0; i < SIZE; i++) {
            list.add(new Integer(i));
        }
    }

    public void writeList() {
	// The FileWriter constructor throws IOException, which must be caught.
        PrintWriter out = new PrintWriter(new FileWriter("OutFile.txt"));

        for (int i = 0; i < SIZE; i++) {
            // The get(int) method throws IndexOutOfBoundsException, which must be caught.
            out.println("Value at: " + i + " = " + list.get(i));
        }
        out.close();
    }
}

```

粗体的第一行是对构造函数的调用。构造函数在文件上初始化一个输出流。如果文件无法打开，构造函数会抛出一个`IOException`异常。第二行粗体是对`ArrayList`类的`get`方法的调用，如果其参数的值太小（小于 0）或太大（大于`ArrayList`当前包含的元素数量），则会抛出`IndexOutOfBoundsException`异常。

如果尝试编译``ListOfNumbers``类，编译器会打印有关`FileWriter`构造函数抛出的异常的错误消息。但是，它不会显示有关`get`方法抛出的异常的错误消息。原因是构造函数抛出的异常`IOException`是一个已检查异常，而`get`方法抛出的异常`IndexOutOfBoundsException`是一个未检查异常。

现在您熟悉了`ListOfNumbers`类以及其中可能抛出异常的位置，您可以编写异常处理程序来捕获和处理这些异常了。
