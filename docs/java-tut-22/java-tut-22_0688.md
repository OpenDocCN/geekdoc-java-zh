# 使用`Sequencer`方法

> 原文：[`docs.oracle.com/javase/tutorial/sound/MIDI-seq-methods.html`](https://docs.oracle.com/javase/tutorial/sound/MIDI-seq-methods.html)

[`Sequencer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequencer.html)接口提供了几个类别的方法：

+   从 MIDI 文件或`Sequence`对象加载序列数据，并将当前加载的序列数据保存到 MIDI 文件。

+   类似于磁带录音机的传输功能的方法，用于停止和开始播放和录制，启用和禁用特定轨道上的录制，并在`Sequence`中快进/快退当前播放或录制位置。

+   高级方法用于查询和设置对象的同步和定时参数。`Sequencer`可以以不同的速度播放，一些`Tracks`静音，并且与其他对象处于各种同步状态。

+   高级方法用于注册“监听器”对象，当`Sequencer`处理某些类型的 MIDI 事件时通知它们。

无论您将调用哪些`Sequencer`方法，第一步都是从系统获取`Sequencer`设备并为程序使用保留它。

## 获取一个`Sequencer`

应用程序不会实例化`Sequencer`；毕竟，`Sequencer`只是一个接口。相反，像 Java Sound API 的 MIDI 包中的所有设备一样，`Sequencer`通过静态的`MidiSystem`对象访问。如前面在访问 MIDI 系统资源中提到的，可以使用以下`MidiSystem`方法获取默认的`Sequencer`：

```java
static Sequencer getSequencer()

```

以下代码片段获取默认的`Sequencer`，获取其所需的任何系统资源，并使其可操作：

```java
Sequencer sequencer;
// Get default sequencer.
sequencer = MidiSystem.getSequencer(); 
if (sequencer == null) {
    // Error -- sequencer device is not supported.
    // Inform user and return...
} else {
    // Acquire resources and make operational.
    sequencer.open();
}

```

调用`open`保留了`Sequencer`设备供程序使用。想象共享一个`Sequencer`并没有太多意义，因为它一次只能播放一个序列。当使用完`Sequencer`后，可以通过调用`close`使其可供其他程序使用。

可以按照访问 MIDI 系统资源中描述的方式获取非默认的`Sequencer`。

## 加载一个序列

从系统获取并保留了一个`Sequencer`后，您需要加载`Sequencer`应该播放的数据。有三种典型的方法可以实现这一点：

+   从 MIDI 文件中读取序列数据

+   通过从另一个设备（如 MIDI 输入端口）接收 MIDI 消息实时录制

+   通过向空序列添加轨道和向这些轨道添加`MidiEvent`对象来以编程方式构建它

现在我们将看一下获取序列数据的这种方式中的第一种。 （其他两种方式分别在录制和保存序列和编辑序列下描述。）这种方式实际上包括两种略有不同的方法。一种方法是将 MIDI 文件数据提供给`InputStream`，然后通过`Sequencer.setSequence(InputStream)`直接将其读取到`sequencer`中。使用此方法，您不需要显式创建`Sequence`对象。实际上，`Sequencer`实现甚至可能不会在幕后创建`Sequence`，因为一些`sequencers`具有处理直接从文件中处理数据的内置机制。

另一种方法是显式创建`Sequence`。如果要在播放之前编辑序列数据，则需要使用此方法。使用此方法，您调用`MidiSystem`的重载方法`getSequence`。该方法能够从`InputStream`、`File`或`URL`获取序列。该方法返回一个`Sequence`对象，然后可以将其加载到`Sequencer`中进行播放。在上面的代码摘录中，这是一个从`File`获取`Sequence`对象并将其加载到我们的`sequencer`的示例：

```java
try {
    File myMidiFile = new File("seq1.mid");
    // Construct a Sequence object, and
    // load it into my sequencer.
    Sequence mySeq = MidiSystem.getSequence(myMidiFile);
    sequencer.setSequence(mySeq);
} catch (Exception e) {
   // Handle error and/or return
}

```

与`MidiSystem`的`getSequence`方法一样，`setSequence`可能会抛出`InvalidMidiDataException`，在`InputStream`变体的情况下，还可能会抛出`IOException`，如果遇到任何问题。

## 播放一个序列

使用以下方法可以启动和停止`Sequencer`：

```java
    void start()

```

和

```java
    void stop()

```

`Sequencer.start`方法开始播放序列。请注意，播放从序列中的当前位置开始。使用上面描述的`setSequence`方法加载现有序列会将`sequencer`的当前位置初始化为序列的开头。`stop`方法停止`sequencer`，但不会自动倒带当前`Sequence`。在不重置位置的情况下启动已停止的`Sequence`只是从当前位置恢复播放序列。在这种情况下，`stop`方法充当了暂停操作。但是，在开始播放之前，有各种`Sequencer`方法可将当前序列位置设置为任意值。（我们将在下面讨论这些方法。）

正如前面提到的，`Sequencer`通常具有一个或多个`Transmitter`对象，通过这些对象向`Receiver`发送`MidiMessages`。通过这些`Transmitters`，`Sequencer`播放`Sequence`，通过发出与当前`Sequence`中包含的`MidiEvents`相对应的适时`MidiMessages`。因此，播放`Sequence`的设置过程的一部分是在`Sequencer`的`Transmitter`对象上调用`setReceiver`方法，实际上将其输出连接到将使用播放数据的设备。有关`Transmitters`和`Receivers`的更多详细信息，请参考传输和接收 MIDI 消息。

## 录制和保存序列

要将 MIDI 数据捕获到`Sequence`，然后保存到文件，需要执行一些除上述描述之外的额外步骤。以下概述显示了设置录制到`Sequence`中的`Track`所需的步骤：

1.  使用`MidiSystem.getSequencer`获取一个新的用于录制的序列器，如上所述。

1.  设置 MIDI 连接的“连线”。传输要录制的 MIDI 数据的对象应通过其`setReceiver`方法配置，以将数据发送到与录制`Sequencer`相关联的`Receiver`。

1.  创建一个新的`Sequence`对象，用于存储录制的数据。创建`Sequence`对象时，必须为序列指定全局时间信息。例如：

    ```java
          Sequence mySeq;
          try{
              mySeq = new Sequence(Sequence.PPQ, 10);
          } catch (Exception ex) { 
              ex.printStackTrace(); 
          }

    ```

    `Sequence`的构造函数接受`divisionType`和时间分辨率作为参数。`divisionType`参数指定分辨率参数的单位。在这种情况下，我们指定正在创建的`Sequence`的时间分辨率为每四分音符 10 脉冲。`Sequence`构造函数的另一个可选参数是轨道数参数，这将导致初始序列以指定数量的（最初为空）`Tracks`开始。否则，`Sequence`将创建为没有初始`Tracks`；它们可以根据需要随后添加。

1.  在`Sequence`中创建一个空的`Track`，使用`Sequence.createTrack`。如果`Sequence`是使用初始`Tracks`创建的，则此步骤是不必要的。

1.  使用`Sequencer.setSequence`，选择我们的新`Sequence`来接收录制。`setSequence`方法将现有的`Sequence`与`Sequencer`绑定，这在某种程度上类似于将磁带加载到磁带录音机上。

1.  对于每个要录制的`Track`，调用`Sequencer.recordEnable`。如果需要，在`Sequence`中通过调用`Sequence.getTracks`获取可用的`Tracks`的引用。

1.  在`Sequencer`上调用`startRecording`。

1.  完成录制后，调用`Sequencer.stop`或`Sequencer.stopRecording`。

1.  使用`MidiSystem.write`将录制的`Sequence`保存到 MIDI 文件中。`MidiSystem`的`write`方法将`Sequence`作为其参数之一，并将该`Sequence`写入流或文件。

## 编辑序列

许多应用程序允许通过从文件加载来创建序列，并且有相当多的应用程序也允许通过从实时 MIDI 输入（即录制）捕获来创建序列。然而，一些程序将需要从头开始创建 MIDI 序列，无论是以编程方式还是响应用户输入。功能齐全的序列程序允许用户手动构建新序列，以及编辑现有序列。

在 Java Sound API 中，这些数据编辑操作不是通过`Sequencer`方法实现的，而是通过数据对象本身的方法实现：`Sequence`、`Track`和`MidiEvent`。你可以使用`Sequence`构造函数之一创建一个空序列，然后通过调用以下`Sequence`方法向其添加轨道：

```java
    Track createTrack() 

```

如果你的程序允许用户编辑序列，你将需要这个`Sequence`方法来移除轨道：

```java
    boolean deleteTrack(Track track) 

```

一旦序列包含轨道，你可以通过调用`Track`类的方法来修改轨道的内容。`Track`中包含的`MidiEvents`以`java.util.Vector`的形式存储在`Track`对象中，而`Track`提供了一组方法来访问、添加和移除列表中的事件。`add`和`remove`方法相当直观，用于向`Track`中添加或移除指定的`MidiEvent`。提供了一个`get`方法，它接受一个索引，返回存储在那里的`MidiEvent`。此外，还有`size`和`tick`方法，分别返回轨道中的`MidiEvents`数量和轨道的持续时间，以总`Ticks`数表示。

在将事件添加到轨道之前创建新事件时，当然会使用`MidiEvent`构造函数。要指定或修改嵌入在事件中的 MIDI 消息，可以调用适当的`MidiMessage`子类（`ShortMessage`、`SysexMessage`或`MetaMessage`）的`setMessage`方法。要修改事件应发生的时间，调用`MidiEvent.setTick`。

综合起来，这些低级方法为完整功能的音序器程序所需的编辑功能提供了基础。
