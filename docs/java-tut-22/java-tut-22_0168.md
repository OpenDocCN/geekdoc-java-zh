# 缓冲流

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/buffers.html`](https://docs.oracle.com/javase/tutorial/essential/io/buffers.html)

到目前为止，我们看到的大多数示例都使用*非缓冲*的 I/O。这意味着每个读取或写入请求都直接由底层操作系统处理。这可能会使程序效率大大降低，因为每个这样的请求通常会触发磁盘访问、网络活动或其他相对昂贵的操作。

为了减少这种开销，Java 平台实现了*缓冲* I/O 流。缓冲输入流从称为*缓冲区*的内存区域读取数据；只有当缓冲区为空时才调用本机输入 API。类似地，缓冲输出流将数据写入缓冲区，只有当缓冲区满时才调用本机输出 API。

一个程序可以使用我们已经多次使用的包装习惯将非缓冲流转换为缓冲流，其中非缓冲流对象传递给缓冲流类的构造函数。以下是您可能如何修改`CopyCharacters`示例中的构造函数调用以使用缓冲 I/O 的方式：

```java
inputStream = new BufferedReader(new FileReader("xanadu.txt"));
outputStream = new BufferedWriter(new FileWriter("characteroutput.txt"));

```

有四个缓冲流类用于包装非缓冲流：[`BufferedInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedInputStream.html)和[`BufferedOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedOutputStream.html)创建缓冲字节流，而[`BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)和[`BufferedWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedWriter.html)创建缓冲字符流。

## 刷新缓冲流

在关键点写出缓冲区而不等待其填满通常是有意义的。这被称为*刷新*缓冲区。

一些缓冲输出类支持*自动刷新*，可以通过可选的构造函数参数指定。启用自动刷新时，某些关键事件会导致缓冲区被刷新。例如，一个自动刷新的`PrintWriter`对象会在每次调用`println`或`format`时刷新缓冲区。查看格式化以获取更多关于这些方法的信息。

要手动刷新流，请调用其`flush`方法。`flush`方法对任何输出流都有效，但除非流是缓冲的，否则不会产生任何效果。
