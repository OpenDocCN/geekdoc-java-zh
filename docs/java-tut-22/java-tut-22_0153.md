# finally 块

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/finally.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/finally.html)

`finally`块*总是*在`try`块退出时执行。这确保了即使发生意外异常，`finally`块也会被执行。但`finally`不仅仅用于异常处理，它允许程序员避免清理代码被`return`、`continue`或`break`意外绕过。将清理代码放在`finally`块中始终是一个良好的实践，即使不预期发生异常。

* * *

**注意：**如果 JVM 在执行`try`或`catch`代码时退出，则`finally`块可能不会执行。

* * *

你一直在这里工作的`writeList`方法的`try`块打开了一个`PrintWriter`。程序应该在退出`writeList`方法之前关闭该流。这带来了一个有点复杂的问题，因为`writeList`的`try`块可以以三种方式之一退出。

1.  `new FileWriter`语句失败并抛出`IOException`。

1.  `list.get(i)`语句失败并抛出`IndexOutOfBoundsException`。

1.  一切顺利，`try`块正常退出。

无论`try`块内发生了什么，运行时系统始终执行`finally`块中的语句。因此，这是执行清理操作的完美位置。

以下`writeList`方法的`finally`块清理并关闭`PrintWriter`和`FileWriter`。

```java
finally {
    if (out != null) { 
        System.out.println("Closing PrintWriter");
        out.close(); 
    } else { 
        System.out.println("PrintWriter not open");
    } 
    if (f != null) {
	    System.out.println("Closing FileWriter");
	    f.close();
	}	
} 

```

* * *

**重要：**在关闭文件或恢复资源时，请使用`try-`with-resources 语句而不是`finally`块。以下示例使用`try`-with-resources 语句清理和关闭`writeList`方法的`PrintWriter`和`FileWriter`：

```java
public void writeList() throws IOException {
    try (FileWriter f = new FileWriter("OutFile.txt");
         PrintWriter out = new PrintWriter(f)) {
        for (int i = 0; i < SIZE; i++) {
            out.println("Value at: " + i + " = " + list.get(i));
        }
    }
}

```

`try`-with-resources 语句在不再需要时自动释放系统资源。请参见 try-with-resources 语句。

* * *
