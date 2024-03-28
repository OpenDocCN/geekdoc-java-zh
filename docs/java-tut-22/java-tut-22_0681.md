# 音频捕获

> 原文：[`docs.oracle.com/javase/tutorial/sound/capturing.html`](https://docs.oracle.com/javase/tutorial/sound/capturing.html)

*捕获*指的是从计算机外部获取信号的过程。音频捕获的常见应用是录音，比如将麦克风输入录制到声音文件中。然而，捕获并不等同于录制，因为录制意味着应用程序始终保存正在输入的声音数据。捕获音频的应用程序不一定存储音频。相反，它可能在音频到达时对数据进行处理，比如将语音转录为文本，但在完成对每个缓冲区的处理后立即丢弃每个缓冲区的音频。

如示例包概述中所讨论的，在 Java Sound API 的实现中，典型的音频输入系统包括：

1.  一个输入端口，比如麦克风端口或线路输入端口，将其传入的音频数据输入到：

1.  一个混音器，将输入数据放入其中：

1.  一个或多个目标数据线，应用程序可以从中检索数据。

通常情况下，一次只能打开一个输入端口，但也可以有一个混音器，从多个端口混合音频。另一种情况是一个没有端口的混音器，而是通过网络获取音频输入。

`TargetDataLine`接口在线接口层次结构下简要介绍过。`TargetDataLine`与`SourceDataLine`接口直接类似，后者在播放音频中有详细讨论。回想一下，`SourceDataLine`接口包括：

+   一个`write`方法将音频发送到混音器

+   一个`available`方法，用于确定可以向缓冲区写入多少数据而不会阻塞

同样，`TargetDataLine`包括：

+   一个`read`方法从混音器获取音频

+   一个`available`方法，用于确定可以从缓冲区读取多少数据而不会阻塞

## 设置 TargetDataLine

获取目标数据线的过程在访问音频系统资源中已经描述，但为了方便起见，我们在这里重复一遍：

```java
TargetDataLine line;
DataLine.Info info = new DataLine.Info(TargetDataLine.class, 
    format); // format is an AudioFormat object
if (!AudioSystem.isLineSupported(info)) {
    // Handle the error ... 

}
// Obtain and open the line.
try {
    line = (TargetDataLine) AudioSystem.getLine(info);
    line.open(format);
} catch (LineUnavailableException ex) {
    // Handle the error ... 
}

```

你可以调用`Mixer`的`getLine`方法，而不是`AudioSystem`的。

正如本例所示，一旦获得目标数据线，您可以通过调用`SourceDataLine`方法`open`来为应用程序保留它的使用权，就像在播放音频中描述的那样。`open`方法的单参数版本使线的缓冲区具有默认大小。您可以通过调用两个参数版本根据应用程序的需要设置缓冲区大小：

```java
void open(AudioFormat format, int bufferSize)

```

## 从 TargetDataLine 读取数据

一旦线路打开，它就准备开始捕获数据，但它还没有激活。要实际开始音频捕获，使用`DataLine`的`start`方法。这将开始将输入音频数据传递到线路的缓冲区，以供您的应用程序读取。只有当应用程序准备好从线路读取时，应用程序才应该调用 start；否则，将浪费大量处理时间来填充捕获缓冲区，只会导致溢出（即，丢弃数据）。

要开始从缓冲区检索数据，请调用`TargetDataLine`的`read`方法：

```java
int read(byte[] b, int offset, int length)

```

这种方法尝试将`length`字节的数据读入数组`b`，从数组中的字节位置`offset`开始。该方法返回实际读取的字节数。

与`SourceDataLine`的`write`方法一样，您可以请求比缓冲区实际容量更多的数据，因为该方法会阻塞，直到请求的数据量已经传递，即使您请求了许多缓冲区的数据量。

为了避免在录制过程中使您的应用程序挂起，您可以在循环中调用 read 方法，直到检索到所有音频输入，就像这个例子中所示的那样：

```java
// Assume that the TargetDataLine, line, has already
// been obtained and opened.
ByteArrayOutputStream out  = new ByteArrayOutputStream();
int numBytesRead;
byte[] data = new byte[line.getBufferSize() / 5];

// Begin audio capture.
line.start();

// Here, stopped is a global boolean set by another thread.
while (!stopped) {
   // Read the next chunk of data from the TargetDataLine.
   numBytesRead =  line.read(data, 0, data.length);
   // Save this chunk of data.
   out.write(data, 0, numBytesRead);
}     

```

请注意，在这个例子中，将数据读入的字节数组的大小设置为线路缓冲区大小的五分之一。如果您将其设置为与线路缓冲区一样大，并尝试读取整个缓冲区，您需要在时间上非常准确，因为如果混音器需要在您从中读取数据时向线路传递数据，数据将被丢弃。通过使用线路缓冲区大小的一部分，如此处所示，您的应用程序将更成功地与混音器共享对线路缓冲区的访问。

`TargetDataLine`的`read`方法接受三个参数：一个字节数组，一个数组中的偏移量，以及您想要读取的输入数据的字节数。在这个例子中，第三个参数就是你的字节数组的长度。`read`方法返回实际读取到数组中的字节数。

通常，您会像这个例子中一样在循环中从线路中读取数据。在`while`循环中，每个检索到的数据块都会根据应用程序的适当方式进行处理——在这里，它被写入`ByteArrayOutputStream`。这里没有显示的是使用单独的线程来设置布尔值`stopped`，该值在循环终止时终止。当用户点击停止按钮时，该布尔值的值可能被设置为`true`，并且当监听器从线路接收到`CLOSE`或`STOP`事件时也是如此。监听器对于`CLOSE`事件是必需的，对于`STOP`事件是推荐的。否则，如果线路在没有将`stopped`设置为`true`的情况下以某种方式停止，`while`循环将在每次迭代中捕获零字节，运行速度快，浪费 CPU 周期。一个更全面的代码示例将展示如果捕获再次激活，则重新进入循环。

与源数据线一样，可以排空或清空目标数据线。例如，如果您正在将输入录制到文件中，当用户点击停止按钮时，您可能希望调用`drain`方法。`drain`方法将导致混音器的剩余数据传递到目标数据线的缓冲区。如果您不排空数据，捕获的声音可能在末尾被截断。

也许有一些情况下，您希望清空数据。无论如何，如果您既不清空也不排空数据，数据将保留在混音器中。这意味着当重新开始捕获时，新录音的开头会有一些残留声音，这可能是不希望的。因此，在重新开始捕获之前清空目标数据线可能是有用的。

## 监控线路状态

因为`TargetDataLine`接口扩展了`DataLine`，目标数据线生成事件的方式与源数据线相同。您可以注册一个对象，以便在目标数据线打开、关闭、启动或停止时接收事件。有关更多信息，请参阅之前关于监控线路状态的讨论。

## 处理传入音频

像一些源数据线一样，一些混音器的目标数据线具有信号处理控件，例如增益、声像、混响或采样率控件。输入端口可能具有类似的控件，特别是增益控件。在下一节中，您将学习如何确定一条线是否具有这些控件，以及如何在有这些控件的情况下使用它们。
