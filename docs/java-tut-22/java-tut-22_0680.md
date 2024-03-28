# 播放音频

> 原文：[`docs.oracle.com/javase/tutorial/sound/playing.html`](https://docs.oracle.com/javase/tutorial/sound/playing.html)

播放有时被称为*演示*或*渲染*。这些是适用于声音以外的其他媒体的通用术语。其关键特征是一系列数据被传送到某个地方，最终由用户感知。如果数据是基于时间的，如声音，它必须以正确的速率传送。与视频相比，对于声音来说，数据流速率的维持更为重要，因为声音播放中断通常会产生响亮的点击声或刺耳的失真。Java Sound API 旨在帮助应用程序平稳连续地播放声音，即使是非常长的声音。

之前您已经看到如何从音频系统或混音器中获取线路。在这里，您将学习如何通过线路播放声音。

如您所知，有两种可以用于播放声音的线路：[`Clip`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Clip.html)和[`SourceDataLine`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/SourceDataLine.html)。两者之间的主要区别在于，使用`Clip`时，您在播放之前一次性指定所有声音数据，而使用`SourceDataLine`时，在播放过程中持续写入新的数据缓冲区。虽然有许多情况可以使用`Clip`或`SourceDataLine`，但以下标准有助于确定哪种线路更适合特定情况：

+   当您有非实时声音数据可以预加载到内存中时，请使用`Clip`。

    例如，您可以将短声音文件读入剪辑中。如果您希望声音重复播放多次，则`Clip`比`SourceDataLine`更方便，特别是如果您希望播放循环（重复通过声音的全部或部分）。如果您需要在声音中的任意位置开始播放，`Clip`接口提供了一种轻松实现的方法。最后，与从`SourceDataLine`缓冲播放相比，从`Clip`播放通常具有更少的延迟。换句话说，因为声音已经预加载到剪辑中，播放可以立即开始，而不必等待缓冲区填充。

+   使用`SourceDataLine`来流式传输数据，比如无法一次性全部放入内存的长声音文件，或者在播放之前无法预先知道数据的声音。

    作为后一种情况的示例，假设您正在监视声音输入，即在捕获声音时播放声音。如果您没有一个可以将输入音频发送回输出端口的混音器，您的应用程序将不得不获取捕获的数据并将其发送到音频输出混音器。在这种情况下，使用`SourceDataLine`比使用`Clip`更合适。另一个无法事先知道的声音示例是当您根据用户的输入合成或操作声音数据时。例如，想象一个游戏通过在用户移动鼠标时从一个声音“变形”到另一个声音来提供听觉反馈。声音转换的动态性要求应用程序在播放过程中持续更新声音数据，而不是在播放开始之前提供所有数据。

## 使用 Clip

如前所述，在获取所需类型的行下获取一个`Clip`；用`Clip.class`作为第一个参数构造一个`DataLine.Info`对象，并将此`DataLine.Info`作为参数传递给`AudioSystem`或`Mixer`的`getLine`方法。

获取一条线只是意味着您已经找到了一个引用它的方法；`getLine`实际上并没有为您保留该线路。因为混音器可能只有有限数量的所需类型的线路可用，所以在您调用`getLine`获取剪辑后，可能会发生另一个应用程序在您准备开始播放之前抢走剪辑的情况。要实际使用剪辑，您需要通过调用以下`Clip`方法之一来为您的程序独占地保留它：

```java
void open(AudioInputStream stream)
void open(AudioFormat format, byte[] data, int offset, int bufferSize)

```

尽管上述第二个`open`方法中有`bufferSize`参数，但`Clip`（不像`SourceDataLine`）不包括用于向缓冲区写入新数据的方法。这里的`bufferSize`参数只指定要加载到剪辑中的字节数组的大小。它不是一个可以随后加载更多数据的缓冲区，就像您可以使用`SourceDataLine`的缓冲区一样。

打开剪辑后，您可以使用`Clip`的`setFramePosition`或`setMicroSecondPosition`方法指定数据中应开始播放的位置。否则，它将从开头开始。您还可以使用`setLoopPoints`方法配置循环播放。

当您准备开始播放时，只需调用`start`方法。要停止或暂停剪辑，请调用`stop`方法，要恢复播放，请再次调用`start`。剪辑会记住停止播放的媒体位置，因此不需要显式的暂停和恢复方法。如果您不希望它在停止播放的位置继续播放，可以使用上述提到的帧或微秒定位方法将剪辑“倒带”到开头（或任何其他位置）。

可以通过调用`DataLine`方法`getLevel`和`isActive`来监视`Clip`的音量级别和活动状态（活动与非活动）。活动的`Clip`是当前正在播放声音的`Clip`。

## 使用 SourceDataLine

获取`SourceDataLine`类似于获取`Clip`。打开`SourceDataLine`也类似于打开`Clip`，因为目的再次是为了保留该线路。但是，您使用从`DataLine`继承的不同方法：

```java
void open(AudioFormat format)

```

请注意，当您打开`SourceDataLine`时，尚未将任何声音数据与该线路关联，与打开`Clip`不同。相反，您只需指定要播放的音频数据的格式。系统会选择默认的缓冲区长度。

您还可以使用以下变体指定特定的字节缓冲区长度：

```java
void open(AudioFormat format, int bufferSize)

```

为了与类似方法保持一致，`bufferSize`参数以字节表示，但必须对应于整数帧数。

除了使用上述描述的 open 方法，还可以使用`Line`的`open()`方法打开`SourceDataLine`，而无需参数。在这种情况下，该线路将以其默认音频格式和缓冲区大小打开。但是，您以后无法更改这些。如果您想知道线路的默认音频格式和缓冲区大小，甚至在线路尚未打开之前，可以调用`DataLine`的`getFormat`和`getBufferSize`方法。

一旦`SourceDataLine`打开，您就可以开始播放声音。您可以通过调用`DataLine`的 start 方法来实现这一点，然后将数据重复写入线路的播放缓冲区。

start 方法允许线路在其缓冲区中有任何数据时开始播放声音。您可以通过以下方法将数据放入缓冲区：

```java
int write(byte[] b, int offset, int length)

```

数组中的偏移量以字节表示，数组的长度也是以字节表示。

该线路尽快将数据发送到其混音器。当混音器将数据传递给其目标时，`SourceDataLine`会生成`START`事件。 （在 Java Sound API 的典型实现中，源线将数据传递给混音器的时间延迟与混音器将数据传递给其目标的时间延迟可以忽略不计，即远小于一个样本的时间。）此`START`事件将发送给线路的侦听器，如下所述监视线路状态。现在该线路被视为活动的，因此`DataLine`的`isActive`方法将返回`true`。请注意，所有这些仅在缓冲区包含要播放的数据时才会发生，不一定在调用 start 方法时立即发生。如果您在新的`SourceDataLine`上调用了`start`但从未向缓冲区写入数据，则该线路永远不会处于活动状态，并且`START`事件永远不会发送。（但是，在这种情况下，`DataLine`的`isRunning`方法将返回`true`。）

那么你如何知道要向缓冲区写入多少数据，以及何时发送第二批数据呢？幸运的是，你不需要计时第二次调用 write 以与第一个缓冲区的结束同步！相反，你可以利用`write`方法的阻塞行为：

+   该方法在数据被写入缓冲区后立即返回。它不会等到缓冲区中的所有数据都播放完毕。（如果等待的话，你可能没有时间写入下一个缓冲区，从而导致音频中断。）

+   尝试写入的数据量超过缓冲区容量是可以的。在这种情况下，该方法会阻塞（不返回），直到你请求的所有数据实际上都被放入缓冲区中。换句话说，每次只会写入一个缓冲区的数据并进行播放，直到剩余数据全部适应缓冲区为止，此时该方法才会返回。无论该方法是否阻塞，它都会在此次调用中最后一个缓冲区的数据被写入时立即返回。这意味着你的代码很可能在最后一个缓冲区的数据播放完成之前就重新获得了控制权。

+   在许多情况下，写入的数据量超过缓冲区容量是可以的，但如果你想确保下一个写入不会阻塞，你可以将写入的字节数限制为`DataLine`的`available`方法返回的数量。

下面是一个示例，迭代从流中读取的数据块，一次将一个数据块写入`SourceDataLine`进行播放：

```java
// read chunks from a stream and write them to a source data 
line 
line.start();
while (total < totalToRead && !stopped)}
    numBytesRead = stream.read(myData, 0, numBytesToRead);
    if (numBytesRead == -1) break;
    total += numBytesRead; 
    line.write(myData, 0, numBytesRead);

}

```

如果你不希望`write`方法阻塞，你可以首先在循环内调用`available`方法来查找可以无阻塞写入的字节数，然后在从流中读取之前将`numBytesToRead`变量限制为这个数字。然而，在给定的示例中，阻塞并不重要，因为 write 方法是在一个循环内调用的，直到最后一个缓冲区在最后一个循环迭代中被写入。无论你是否使用阻塞技术，你可能会希望在应用程序的其余部分之外的一个单独线程中调用这个播放循环，这样当播放长声音时，你的程序不会出现冻结的情况。在循环的每次迭代中，你可以测试用户是否请求停止播放。这样的请求需要将上面代码中使用的`stopped`布尔值设置为`true`。

由于`write`在所有数据完成播放之前返回，那么你如何知道播放实际上已经完成了呢？一种方法是在写入最后一个缓冲区数据后调用`DataLine`的`drain`方法。该方法会阻塞，直到所有数据都已经播放完毕。当控制返回到你的程序时，如果需要的话，你可以释放该线路，而不必担心会过早中断任何音频样本的播放：

```java
line.write(b, offset, numBytesToWrite); 
//this is the final invocation of write
line.drain();
line.stop();
line.close();
line = null;

```

你可以有意提前停止播放，当然。例如，应用程序可能会为用户提供一个停止按钮。调用`DataLine`的`stop`方法可以立即停止播放，即使在缓冲区的中间。这会使缓冲区中的任何未播放数据保留下来，因此如果随后调用`start`，播放将从停止的地方恢复。如果这不是你想要发生的事情，你可以通过调用`flush`来丢弃缓冲区中剩余的数据。

当数据流的流动已停止时，`SourceDataLine`会生成一个`STOP`事件，无论此停止是由`drain`方法、`stop`方法、`flush`方法引起的，还是因为在应用程序调用`write`提供新数据之前已到达播放缓冲区的末尾。`STOP`事件并不一定意味着已调用`stop`方法，并且也不一定意味着随后调用`isRunning`将返回`false`。但是，它确实意味着`isActive`将返回`false`。（当调用`start`方法时，即使生成`STOP`事件，`isRunning`方法也将返回`true`，并且只有在调用`stop`方法后才会开始返回`false`。）重要的是要意识到`START`和`STOP`事件对应于`isActive`，而不是`isRunning`。

## 监控线路的状态

一旦开始播放声音，如何确定何时完成？我们在上面看到了一种解决方案，在写入最后一个数据缓冲区后调用`drain`方法，但这种方法仅适用于`SourceDataLine`。另一种途径，适用于`SourceDataLines`和`Clips`，是注册以接收线路在改变其状态时发出的通知。这些通知以`LineEvent`对象的形式生成，其中有四种类型：`OPEN`、`CLOSE`、`START`和`STOP`。

在程序中实现`LineListener`接口的任何对象都可以注册以接收此类通知。要实现`LineListener`接口，对象只需要一个接受`LineEvent`参数的`update`方法。要将此对象注册为线路的侦听器之一，您需要调用以下`Line`方法：

```java
public void addLineListener(LineListener listener)

```

每当线路打开、关闭、启动或停止时，它会向所有侦听器发送一个`update`消息。您的对象可以查询接收到的`LineEvent`。首先，您可能会调用`LineEvent.getLine`来确保停止的线路是您关心的线路。在我们讨论的情况下，您想知道声音是否已完成，因此您可以查看`LineEvent`是否为`STOP`类型。如果是，您可以检查声音的当前位置，该位置也存储在`LineEvent`对象中，并将其与声音的长度（如果已知）进行比较，以查看是否已达到结束并且没有被其他方式停止（例如用户点击停止按钮，尽管您可能能够在代码的其他地方确定该原因）。

同样，如果您需要知道何时打开、关闭或启动线路，可以使用相同的机制。`LineEvents`由不同类型的线路生成，不仅仅是`Clips`和`SourceDataLines`。但是，在`Port`的情况下，您不能指望获得事件来了解线路的打开或关闭状态。例如，当创建`Port`时，可能会最初打开`Port`，因此您不会调用`open`方法，而`Port`也不会生成`OPEN`事件。 (请参阅之前关于选择输入和输出端口的讨论。)

## 同步多条线路的播放

如果您同时播放多个音轨，您可能希望它们都在完全相同的时间开始和停止。一些混音器通过其`synchronize`方法促进此行为，该方法允许您对一组数据线应用操作，如`open`、`close`、`start`和`stop`，而不是必须单独控制每条线路。此外，可以控制对线路应用操作的精度程度。

要了解特定数据线组的特定混音器是否提供此功能，调用`Mixer`接口的`isSynchronizationSupported`方法：

```java
boolean isSynchronizationSupported(Line[] lines, boolean  maintainSync)

```

第一个参数指定了一组特定的数据线，第二个参数表示必须保持同步的精度。如果第二个参数是`true`，则查询是在询问混音器是否能够始终在*所有时间*内保持对指定线路的样本精确控制；否则，精确同步仅在启动和停止操作期间需要，而不是在整个播放过程中。

## 处理传出音频

一些源数据线具有信号处理控件，如增益、声像、混响和采样率控件。类似的控件，尤其是增益控件，也可能存在于输出端口上。有关如何确定一条线路是否具有此类控件以及如何在有此类控件的情况下使用它们的更多信息，请参阅使用控件处理音频。
